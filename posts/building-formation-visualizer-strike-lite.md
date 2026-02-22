# Building a Formation Visualizer for Strike Lite: Bridging Normalized Coordinates to Canvas

**Published:** February 22, 2026 | **Category:** iOS Development

---

I've been working on improving team management in Strike Lite, my soccer coaching app. One of the key features is letting coaches visualize and customize formations—4-4-2, 4-3-3, 3-5-2, and more. The challenge wasn't defining the formations; it was rendering them at any screen size without hardcoding pixels.

## The Problem I Faced

My formation data lives in normalized space (0 to 1). The pitch is abstract—no width or height. But when I render on screen, I need real pixels. How do I scale from abstract to concrete without breaking responsiveness?

## My Approach: Normalized Coordinates

I defined each player position using proportional coordinates:

```swift
struct FormationPosition: Identifiable {
    let id: UUID
    let number: Int
    let abbreviation: String
    let x: Double      // 0.0 to 1.0 (left to right)
    let y: Double      // 0.0 to 1.0 (attacking to own goal)
}
```

A defender at `(0.16, 0.78)` means "16% from the left, 78% down from the attacking goal." Same position, any screen size.

## Rendering with Canvas

Once I had the positions, I used SwiftUI's `Canvas` to render them:

```swift
struct FormationCanvas: View {
    let formation: Formation
    let layout: PitchLayout

    var body: some View {
        Canvas { context, size in
            let positions = formation.positions(for: layout)

            // Green pitch
            context.fill(
                Path(roundedRect: CGRect(origin: .zero, size: size), cornerRadius: 8),
                with: .color(.green.opacity(0.3))
            )

            // Render each player pin
            for position in positions {
                let screenX = position.x * size.width
                let screenY = position.y * size.height

                // Draw circle
                let pinRect = CGRect(x: screenX - 15, y: screenY - 15, width: 30, height: 30)
                context.fill(Path(ellipseIn: pinRect), with: .color(.blue))

                // Draw label
                context.draw(
                    Text(position.abbreviation)
                        .font(.system(size: 10, weight: .bold))
                        .foregroundColor(.white),
                    at: CGPoint(x: screenX, y: screenY),
                    anchor: .center
                )
            }
        }
        .aspectRatio(5/7, contentMode: .fit)
    }
}
```

## The Core Insight

```swift
let screenX = position.x * size.width
let screenY = position.y * size.height
```

That's the entire bridge. I multiply normalized coordinates by the actual canvas dimensions. A 4-4-2 on iPhone 15 and iPad Pro look identical—just scaled.

## Why This Approach Works

1. **Formation logic is pure:** No UI dependencies, easy to test
1. **Responsive by design:** Canvas resizes, pins scale automatically
1. **Separation of concerns:** Domain (formations) stays independent from presentation (rendering)

## What's Next

This foundation lets me add interaction—drag to reposition players, tap to select, animate transitions between formations. All without touching coordinate math.

The normalized coordinate system scaled to the real world. Simple, flexible, and coach-friendly.
