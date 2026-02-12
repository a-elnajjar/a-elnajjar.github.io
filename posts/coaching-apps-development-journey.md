# The Journey of Building Sports Coaching Apps

**Published:** March 1, 2025 | **Category:** Behind the Scenes

---

When I first started building **Strike Soccer Coach**, I had no idea it would lead to an entire suite of coaching tools. Here's the story of how a simple idea grew into a family of apps used by coaches around the world.

## The Spark

It started on a rainy Saturday morning at a youth soccer practice. The coach was trying to explain a formation using a whiteboard that kept getting wet. I thought, "There has to be a better way to do this on a phone."

That weekend, I sketched out the first prototype of what would become **Strike Lite** — a simple tactical whiteboard for soccer.

## From Prototype to Product

### Version 1: The MVP

The first version was bare-bones:
- A green rectangle (the pitch)
- Draggable circles (the players)
- A save button

It took about two weeks to build, and honestly, it wasn't great. But coaches who tried it immediately saw the potential.

### Version 2: Adding Real Value

Based on early feedback, I added:
- **Custom formations** — preset and custom player arrangements
- **Drawing tools** — arrows, lines, and zones for tactical instructions
- **Export functionality** — share tactics as images with your team
- **Multiple pitch views** — full pitch, half pitch, and set piece views

### Version 3: The Full Suite

The feedback from soccer coaches was so positive that basketball coaches started asking for a similar tool. That's how **Hoop Basketball Coach** was born.

## Technical Challenges

### Challenge 1: Smooth Drawing on Canvas

Getting smooth, responsive drawing on a touch screen is harder than it looks. The key was using **quadratic Bezier curves** instead of straight line segments:

```swift
// Smooth drawing using quadratic curves
func drawStroke(_ stroke: Stroke) {
    let path = UIBezierPath()
    guard stroke.points.count > 1 else { return }

    path.move(to: stroke.points[0])

    for i in 1..<stroke.points.count {
        let midPoint = CGPoint(
            x: (stroke.points[i-1].x + stroke.points[i].x) / 2,
            y: (stroke.points[i-1].y + stroke.points[i].y) / 2
        )
        path.addQuadCurve(to: midPoint, controlPoint: stroke.points[i-1])
    }
}
```

### Challenge 2: Player Positioning

Making player icons draggable while also supporting drawing mode required careful gesture management. We ended up implementing a **mode-switching** system where the canvas toggles between "edit" and "draw" modes.

### Challenge 3: Cross-Sport Adaptation

Each sport has unique requirements:

| Feature | Soccer | Basketball |
|---------|--------|------------|
| Field Shape | Rectangle | Rectangle |
| Player Count | 11 | 5 |
| Field Markings | Center circle, penalty area, goals | Three-point line, free throw, key |
| Common Plays | Set pieces, formations | Pick and roll, fast break |

We built a **sport-agnostic core engine** that handles drawing, player management, and export, with sport-specific overlays for field markings and default formations.

## Lessons Learned

1. **Talk to your users** — Every major feature in our apps came from direct user feedback
2. **Start simple** — Strike Lite's simplicity is actually its biggest selling point
3. **Privacy matters** — Coaches appreciate that their tactics stay on their device
4. **Iterate fast** — Ship the MVP, gather feedback, improve, repeat
5. **Cross-platform thinking** — Success in one sport opened doors to others

## What's Next

We're currently working on:
- **ASTRO Drawing Board** — A general-purpose sketching app (coming soon!)
- Enhanced export options including PDF and animated playbook sequences
- Apple TV support for presenting tactics on the big screen during team meetings

## The Bigger Picture

Building these coaching apps reinforced my belief that the best software solves real problems for real people. You don't need AI, blockchain, or the latest buzzword technology. Sometimes, a well-designed drag-and-drop interface is all it takes to make someone's day better.

---

*Are you a coach with ideas for features? I'd love to hear from you at [alex_elnajjar@outlook.com](mailto:alex_elnajjar@outlook.com).*
