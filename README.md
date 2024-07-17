# Font-Size-Manager-Mac-App
Code for managing font sizes in a Mac app using Swift
# Font Size Manager for Mac App

This repository contains the code to manage font sizes in your Mac app using Swift. Follow these steps to integrate it into your project.

## Step-by-Step Integration Guide

### Step 1: Add `FontManager.swift` to Your Project

1. **Create a New Swift File**:
   - Open your Xcode project.
   - Right-click on the group where you want to add the new file (e.g., `Models`, `Utilities`).
   - Select `New File...`.
   - Choose `Swift File` and click `Next`.
   - Name the file `FontManager.swift` and click `Create`.

2. **Add the Code**:
   - Copy and paste the following code into `FontManager.swift`:

     ```swift
     import SwiftUI

     enum FontSize: String, CaseIterable {
         case small
         case medium
         case large
         
         var size: CGFloat {
             switch self {
             case .small:
                 return 14
             case .medium:
                 return 16
             case .large:
                 return 18
             }
         }
     }

     class FontSizeViewModel: ObservableObject {
         @Published var selectedFontSize: FontSize = .medium
     }
     ```

### Step 2: Add `FontSizeEnvironment.swift` to Your Project

1. **Create a New Swift File**:
   - Right-click on the group where you want to add the new file.
   - Select `New File...`.
   - Choose `Swift File` and click `Next`.
   - Name the file `FontSizeEnvironment.swift` and click `Create`.

2. **Add the Code**:
   - Copy and paste the following code into `FontSizeEnvironment.swift`:

     ```swift
     import SwiftUI

     struct FontSizeKey: EnvironmentKey {
         static let defaultValue: FontSize = .medium
     }

     extension EnvironmentValues {
         var fontSize: FontSize {
             get { self[FontSizeKey.self] }
             set { self[FontSizeKey.self] = newValue }
         }
     }

     struct DynamicFontSize: ViewModifier {
         @Environment(\.fontSize) var fontSize: FontSize
         
         func body(content: Content) -> some View {
             content.font(.system(size: fontSize.size))
         }
     }

     extension View {
         func dynamicFontSize() -> some View {
             self.modifier(DynamicFontSize())
         }
     }
     ```

### Step 3: Add `FontSizeSelectionView.swift` to Your Project

1. **Create a New Swift File**:
   - Right-click on the group where you want to add the new file.
   - Select `New File...`.
   - Choose `Swift File` and click `Next`.
   - Name the file `FontSizeSelectionView.swift` and click `Create`.

2. **Add the Code**:
   - Copy and paste the following code into `FontSizeSelectionView.swift`:

     ```swift
     import SwiftUI

     struct FontSizeSelector: View {
         @EnvironmentObject var viewModel: FontSizeViewModel
         @Environment(\.colorScheme) var colorScheme
         
         var body: some View {
             Picker("Font Size", selection: $viewModel.selectedFontSize) {
                 ForEach(FontSize.allCases, id: \.self) { fontSize in
                     Text(fontSize.rawValue.capitalized).tag(fontSize)
                         .foregroundColor(colorScheme == .dark ? .white : .init(hex: "000000"))
                 }
                 
             }
             .background(colorScheme == .dark ? Color.init(hex: "3C4146") : .init(hex: "E6E6EE"))
             .pickerStyle(MenuPickerStyle())
             .frame(width: 80, height: 40)
             .cornerRadius(8)
             .padding()
         }
     }
     ```

### Step 4: Update Your Views

1. **Use Dynamic Font Sizes**:
   - In your SwiftUI views, use the `dynamicFontSize` modifier to apply the selected font size.

     ```swift
     struct ContentView: View {
         @EnvironmentObject var fontSizeViewModel: FontSizeViewModel

         var body: some View {
             VStack {
                 FontSizeSelector()
                     .environmentObject(fontSizeViewModel)
                 Text("Sample Text")
                     .dynamicFontSize()
             }
             .environment(\.fontSize, fontSizeViewModel.selectedFontSize)
         }
     }
     ```

### Step 5: Test Your Font Size Selection

1. **Run Your App**:
   - Build and run your app in Xcode.
   - Use the font size picker to switch between font sizes and ensure the text size changes accordingly.

### Conclusion

By following these steps, you can integrate a dynamic font size manager into your Mac app, providing a better user experience with customizable text sizes. For more details and to access the code, visit my [GitHub Repository](https://github.com/Hafiz467/Font-Size-Manager-Mac-App).

Feel free to check it out, contribute, or provide feedback. Happy coding!

Best regards,  
Hafiz Muhammad Hamad
