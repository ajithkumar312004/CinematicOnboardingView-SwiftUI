Cinematic Onboarding View — Smooth SwiftUI Motion Flow
[![Releases](https://img.shields.io/github/v/release/ajithkumar312004/CinematicOnboardingView-SwiftUI)](https://github.com/ajithkumar312004/CinematicOnboardingView-SwiftUI/releases)

![Cinematic Onboarding Hero](https://images.unsplash.com/photo-1519999482648-25049ddd37b1?auto=format&fit=crop&w=1600&q=80)

A cinematic onboarding flow built in SwiftUI that blends motion, layering, and typography. Use it to create an immersive first-run experience in iOS apps. The package provides ready-made views, transitions, and a small state machine to control step flow.

Table of contents
- Features
- Demo
- Requirements
- Installation (Download and Execute)
- Quick start
- API overview
- Customization
- Design patterns used
- Accessibility and performance
- Testing
- Contributing
- License
- Credits

Features
- Multi-step cinematic scenes with parallax and blur.
- Smooth animated transitions using matchedGeometryEffect and custom transitions.
- Content layout builders for text, media, and CTA buttons.
- Progress control hooks and automatic step timers.
- Light and dark theme support.
- Swift Package Manager ready.

Demo
- Live demo GIF (inline)
![Demo GIF](https://raw.githubusercontent.com/ajithkumar312004/CinematicOnboardingView-SwiftUI/main/Assets/demo/demo-gif.gif)

Requirements
- Xcode 14 or later
- iOS 15 or later (SwiftUI 3)
- Swift 5.7 or later

Installation (Download and Execute)
- You can get a prebuilt release from the Releases page:
  https://github.com/ajithkumar312004/CinematicOnboardingView-SwiftUI/releases
- Download the release asset named CinematicOnboardingView-SwiftUI.zip and execute the sample app inside the archive:
  1. Download the .zip file from the Releases link above.
  2. Unzip the archive.
  3. Open CinematicOnboardingView-SwiftUI.xcodeproj or the sample workspace.
  4. Select a simulator or device and run the sample app.
- Or add the package via Swift Package Manager:
  1. In Xcode choose File > Add Packages...
  2. Enter the repository URL: https://github.com/ajithkumar312004/CinematicOnboardingView-SwiftUI
  3. Choose the package product and add it to your app target.

Quick start
- Add the package to your project via SPM or paste the source files.
- Import the module and add a CinematicOnboardingView instance.

Example
```swift
import SwiftUI
import CinematicOnboardingView

struct OnboardingExample: View {
    @State private var showOnboarding = true

    var body: some View {
        ZStack {
            MainAppView()
            if showOnboarding {
                CinematicOnboardingView(steps: sampleSteps) {
                    showOnboarding = false
                }
                .transition(.opacity)
            }
        }
    }
}

let sampleSteps: [CinematicStep] = [
    .init(id: "welcome",
          title: "Welcome to Nova",
          subtitle: "A new way to explore content",
          imageName: "space-bg",
          backgroundColor: .black),
    .init(id: "features",
          title: "Motion and Depth",
          subtitle: "Parallax layers bring screens to life",
          imageName: "depth-bg",
          backgroundColor: .purple)
]
```

API overview
- CinematicOnboardingView(steps: [CinematicStep], onFinish: () -> Void)
  - steps: Array of CinematicStep models that define each scene.
  - onFinish: Called when the user completes onboarding.
- CinematicStep
  - id: String
  - title: String
  - subtitle: String
  - imageName: String (asset name or SF Symbol)
  - backgroundColor: Color
  - duration: TimeInterval? (optional auto-advance)
  - layout: StepLayout (enum controlling text and media positions)
- CinematicProgress
  - Bindable object that gives current index, percent, and allows navigation.
- CinematicModifier
  - ViewModifier to apply cinematic headers or overlays to any view.

Customization
- Replace images with high-resolution assets for each device size.
- Use StepLayout to change text alignment, image position, and overlay style.
- Tweak motion strength with CinematicConfig.motionScale.
- Override fonts with the provided FontSet struct:
```swift
CinematicConfig.fonts.heading = .system(size: 28, weight: .semibold)
CinematicConfig.fonts.subheading = .system(size: 16, weight: .regular)
```
- Add custom transitions by passing your own AnyTransition via the transition parameter.

Design patterns used
- State-driven UI: Steps drive the view. The view reacts to step changes.
- Small state machine: Controls auto-advance, user skip, and step rewinds.
- Matched geometry and parallax: For layered motion and smooth visuals.
- Composition: Provide small building blocks (step view, overlay, CTA) rather than a single large class.

Animations and transitions (how it works)
- Each step composes 3 layers: background image, mid layer content, foreground UI.
- The library uses:
  - matchedGeometryEffect for smooth shared element transitions.
  - offset and scale animations for parallax.
  - opacity and blur for depth.
- Timers control auto-advance when duration is set. User input cancels timers.

Accessibility and performance
- VoiceOver labels ship with each step. Provide custom accessibilityLabel on CinematicStep if you need a different description.
- Respect Reduce Motion: When UIAccessibility.isReduceMotionEnabled the view reduces non-essential animation.
- Use compressed image sizes for lower memory use. The library supports progressive loading.
- Use .drawingGroup() sparingly; it can help in some gradients but may cost GPU time.

Testing
- Unit tests cover step progression and state transitions.
- UI tests verify that the onboarding finishes when CTA taps occur and that skip works.
- Run tests with:
  - xcodebuild test -scheme CinematicOnboardingView -destination 'platform=iOS Simulator,name=iPhone 14'

Troubleshooting
- If the sample app fails to run, check the selected device target and the Swift tools version.
- If animations stutter on older devices, reduce motionScale or disable blur effects.

Release notes and downloads
- Stable release artifacts live on the Releases page:
  https://github.com/ajithkumar312004/CinematicOnboardingView-SwiftUI/releases
- Download the named release bundle and execute the sample app bundle or the example binary included in the release assets. The release contains:
  - Source zip
  - Compiled sample app for simulator
  - Demo GIF and screenshots
- The Releases page also lists change logs for each version.

Example use cases
- Feature tours for media apps.
- Product walkthroughs for lifestyle apps.
- Campaign launches where immersive visuals help conversion.
- App onboarding where context and motion improve retention.

Files and folder layout
- Sources/
  - CinematicOnboardingView/
    - CinematicView.swift
    - CinematicStep.swift
    - CinematicProgress.swift
    - CinematicConfig.swift
- Example/
  - CinematicOnboardingViewApp/
    - App entry and sample steps
- Assets/
  - Images and demo GIFs
- Tests/
  - Unit and UI tests
- LICENSE

Contributing
- Open an issue for feature requests or bug reports.
- Fork the repo and open a pull request.
- Keep changes small and focused.
- Follow the provided code style in the project.

Changelog highlights (sample)
- v1.2.0 — Add custom layout variants and accessibility improvements.
- v1.1.0 — Add step auto-advance and progress binding.
- v1.0.0 — Initial release with core cinematic engine.

License
- MIT License. See LICENSE file.

Credits
- Built with SwiftUI and inspired by modern cinematic UI patterns.
- Demo images from Unsplash and example assets created for the sample app.

Contact
- Open issues and pull requests on GitHub.

Acknowledgements and inspiration images
- Hero image: Unsplash (photo)
- Motion concepts inspired by film editing and layered parallax systems.

Badges and quick links
[![Download Release](https://img.shields.io/badge/Download-Releases-blue?logo=github)](https://github.com/ajithkumar312004/CinematicOnboardingView-SwiftUI/releases)

License and legal files live in the repository.