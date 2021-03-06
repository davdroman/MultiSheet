# MultiModal

![CI](https://github.com/davdroman/MultiModal/workflows/CI/badge.svg)

## Introduction

By default, SwiftUI views with multiple modal modifiers (e.g. `.sheet`, `.alert`) in the same body will only use the last one in the chain of modifiers and ignore all previous ones.

```swift
struct NoMultiModalDemoView: View {
    @State var sheetAPresented = false
    @State var sheetBPresented = false
    @State var sheetCPresented = false

    var body: some View {
        VStack(spacing: 20) {
            Button("Sheet A") { sheetAPresented = true }
            Button("Sheet B") { sheetBPresented = true }
            Button("Sheet C") { sheetCPresented = true }
        }
        .sheet(isPresented: $sheetAPresented) { Text("Sheet A") } // does not work
        .sheet(isPresented: $sheetBPresented) { Text("Sheet B") } // does not work
        .sheet(isPresented: $sheetCPresented) { Text("Sheet C") } // works
    }
}
```

**MultiModal** brings a `.multiModal` modifier to declare multiple modal modifiers in the same view body.

```swift
struct MultiModalDemoView: View {
    @State var sheetAPresented = false
    @State var sheetBPresented = false
    @State var sheetCPresented = false

    var body: some View {
        VStack(spacing: 20) {
            Button("Sheet A") { sheetAPresented = true }
            Button("Sheet B") { sheetBPresented = true }
            Button("Sheet C") { sheetCPresented = true }
        }
        .multiModal {
            $0.sheet(isPresented: $sheetAPresented) { Text("Sheet A") } // works
            $0.sheet(isPresented: $sheetBPresented) { Text("Sheet B") } // works
            $0.sheet(isPresented: $sheetCPresented) { Text("Sheet C") } // works
        }
    }
}
```

## Try it out!

MultiModal supports [Arena](https://github.com/finestructure/Arena) to effortlessly test this library in a playground before you decide to take it for a spin in your own project.

Simply [install Arena](https://github.com/finestructure/Arena#how-to-install-arena) and run `arena davdroman/MultiModal --platform ios` in your terminal.

Alternatively, a demo Xcode project is provided in the [Demo](Demo) directory.

## Disclaimer

MultiModal does not enable "nested" modals; it just enables multiple modals appearing within a view body **one at a time**. For this reason, it's recommended that your modal presentation be dependant on a source of truth that ensures only one of them is presented at any given time.

Hopefully Apple will introduce support for multiple modals in a future iteration of SwiftUI, rendering this library unnecessary.
