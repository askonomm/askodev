---
title: Rendering a SwiftUI view to multiple A4 PDF pages using ImageRenderer
date: 2023-07-08
slug: multiple-pdf-pages-imagerenderer-swiftui
status: published
---

The current version of [Invobi](https://apps.apple.com/us/app/invobi/id6450777868) is able to create PDF’s using the SwiftUI `ImageRenderer` by rendering an entire SwiftUI view into a PDF file. It does so basically 1:1 with how the [ImageRenderer documentation shows](https://developer.apple.com/documentation/swiftui/imagerenderer#Rendering-to-a-PDF-context) to do it. However, the resulting PDF is always just one page, and the size of it is determined by the size of the view, making it fine for online-only use, but difficult for printing.

I wanted to offer people options to change this. They could continue saving one-page PDF files with fluid sizes or they could choose a fixed format, like A4 or US Letter, which would save multi-page PDF’s, ideal for printing. To do this, I created dimensions for the size of a single PDF page based on the desired format, and then split the view into parts, rendering each part to its corresponding page based on `translateBy(y:)` of the `CGContext`.

## Determine the ideal page size

Before we can do anything else, we should figure out what is the exact page size we want something to have. Let’s start with the format A4, which at 72dpi would be:

```swift
let pageWidth = 8.27 * 72
let pageHeight = 11.69 * 72
```

## Create the renderer

Now that we know what we want each page to look like, let’s create the renderer, inside of which we’ll be doing all the math required to split views and to render them.

```swift
let view = YourSwiftUIView().frame(width: pageWidth)
let renderer = ImageRenderer(content: view)

renderer.render { size, context in
    // here we'll be doing our work
}
```

## Create the PDF pages

Let’s create a `CGRect` and `CGContext`, which we’ll be using for for drawing to our PDF.

```swift
var box = CGRect(x: 0, y: 0, width: pageWidth, height: pageHeight)

guard let pdf = CGContext(fileUrl as CFURL, mediaBox: &box, nil) else {
    return
}
```

Now let’s get a `pageCount` figured out, so we can loop over something to create the correct number of PDF pages per our selected format.

```swift
let totalHeight = size.height
let pageCount = Int(ceil(totalHeight / pageHeight))
```

Yup, that’s it, just divide the total view height by the desired page height. But, make sure to `ceil` that number up, because there most likely will be a case where the last page will only be partially filled, and have more than 50% of empty space, but you still want that partial rendering instead of losing that half a page.

And now, let’s create our PDF pages.

```swift
for page in 0...(pageCount - 1) {
    pdf.beginPDFPage(nil)

    // Here we'll be doing the drawing

    pdf.endPDFPage()
}

pdf.closePDF()
```

When you run the what we’ve so far come up with, you should be getting the correct number of PDF pages based on the A4 format and your SwiftUI view size, but they are all blank.

## Draw to the PDF pages

Since the view is `totalHeight` tall, and our PDF pages are `pageHeight` tall, we need to start positioning our view in our PDF pages in such a way that it’ll start from a point where the previous page left off. To do this, we’ll be using the `CGContext`‘s `translateBy` method, which allows us to to position our PDF page area anywhere in the SwiftUI view we are rendering. Think of it like a magnifying glass only going over a specific portion of a newspaper.

There’s one thing though, which complicates this a bit – it’s that the `translateBy(y:)` goes from bottom to top, not from top to bottom, meaning that we have to do our positioning in reverse, where the last page has the Y coordinate being `0`, and the first page has the Y coordinate `-{totalHeight}`.

Let’s calculate our Y coordinate.

```swift
let y = -(Double(pageCount - 1) - Double(page)) * pageHeight
```

This takes the page count, but deducts 1 so that we start from 0, then takes the current `page`, deducts that from the result as well, and multiplies it by `pageHeight`, thus arriving at a result `-0.0` for our last page and `-totalHeight` for our first page. Remember, we’re going backwards here.

So, putting this together, we can draw our pages like this:

```swift
for page in 0...(pageCount - 1) {
    pdf.beginPDFPage(nil)

    let y = -(Double(pageCount - 1) - Double(page)) * pageHeight

    pdf.translateBy(x: 0, y: CGFloat(y))
    context(pdf)

    pdf.endPDFPage()
}
```

## But wait, that’s not quite right!

You might be seeing some odd extra space on top of the first page. Why isn’t the page starting from the beginning like it should?

![](https://omma.ee/wp-content/uploads/2023/07/Screenshot-2023-07-08-at-15.12.12-1024x815.png)

Well, remember that `ceil` we did way back in the beginning to round upward the number of pages we want? That’s the reason. Because of the `ceil`, we get some extra space we don’t actually use, and because the whole `translateBy(y:)` logic goes backwards, the empty space does not show up in the end of the last page, where it would be fine, but rather in beginning before the first page, where it is not fine.

But, worry not, the fix is rather simple. We first have to figure out what is that extra space, and add it to our `let y` , like this:

```swift
let emptyOffset = (Double(pageCount) * pageHeight) - totalHeight
```

I suggest to do this calculation right before the `for` loop, because we only need to calculate it once and not per page. Then, once that’s done, modify the `let y` like this:

```swift
let y = -(((Double(pageCount - 1) - Double(page)) * pageHeight) - emptyOffset)
```

And that solves that problem. Now you should have a perfectly functioning, multi-page PDF rendering system. If you want to change the format, simply change the `pageWidth` and `pageHeight` variables accordingly, the rest will continue working without needing any changes.