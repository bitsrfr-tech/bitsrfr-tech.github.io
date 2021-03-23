---
layout: post
title: "Fix onTapGesture Not Detecting Taps (SwiftUI)"
date: 2021-02-25
categories: iOS, Swift, SwiftUI, Apple
---

If you are trying to use SwiftUI's .onTapGesture() to run some code when the user taps, and the code is not running, you are not alone. [Many iOS developers have run into this issue](https://developer.apple.com/forums/thread/652258?login=true#reply-to-this-question). I was one them until I came across .simultaneousGesture().

I was trying to use .onTapGesture() on a NavigationLink, and it was not working. Xcode didn't give me any errors or warnings, but the onTapGesture code was not running.

After some digging and experimenting, I realized that NavigationLink views already watch for a tap gesture. That way, when the user taps on the navigation link, it can go to the destination view. Because NavigationLink is already handling the tap gesture, adding .onTapGesture() to it is basically ignored.

This same issue will apply to any SwiftUI view that is already handling tap gestures, but the solution is simple. Use .simultaneousGesture() instead of .onTapGesture(). Using .simultaneousGesture() allows the simultaneous handling of a tap gesture, so even though the view is already defining how the tap gesture is handled, you can define additional code to run on tap.

The first code snippet demonstrates the problem. The second code snippet demonstrates the solution.

Hope this helps!

### Using .onTapGesture() does not work:

{% highlight swift %}
import SwiftUI

struct ContentView: View {
     @State var text: String = "testing"

     var body: some View {
         NavigationView {
            NavigationLink("Button", destination: TextEditor(text: $text)).onTapGesture {
                 print("tapped") /* "tapped" does not print to the console */
             }
         }
     }
 }

 struct ContentView_Previews: PreviewProvider {
     static var previews: some View {
         ContentView()
     }
 }
{% endhighlight %}

### Using .simultaneousGesture() works:
{% highlight swift %}
import SwiftUI

struct ContentView: View {
    @State var text: String = "testing"

    var body: some View {
        NavigationView {
            NavigationLink("Button", destination: TextEditor(text: $text)).simultaneousGesture(TapGesture().onEnded {
                print("tapped")
            })
        }
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}
{% endhighlight %}

### [From Apple Developer Documentation onTapGesture page](https://developer.apple.com/documentation/swiftui/scrollview/ontapgesture(count:perform:)):
func simultaneousGesture<T>(T, including: GestureMask) -> some View
    
Attaches a gesture to the view to process simultaneously with gestures defined by the view.
    
### Additional Resources
- [Stack Overflow post that got me moving in the right direction](https://stackoverflow.com/questions/63422953/swiftui-list-ontapgesture-covered-navigationlink)
- [Apple Developer Documentation SimultaneousGesture page](https://developer.apple.com/documentation/swiftui/simultaneousgesture)
