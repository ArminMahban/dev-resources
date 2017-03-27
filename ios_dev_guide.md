# (Almost) Everything you need to be an Swift iOS developer 

I’ve only included the essentials in this guide. There is certainly more to say about each topic, but after 2 years of iOS development, these are the ideas that I feel are most important.

## Getting Started

### XCode and Setup

Use an external monitor! Xcode uses multiple panes and inspector windows that quickly crowd your actual coding space. Using a large external monitor will alleviate this issue and speed up your workflow considerably. 

Here's the screen layout the has worked for me:

TODO: Insert picture

Xcode also has a really nice git interface that’s easy to use and can help you handle commits and merges. Get started with that in the ‘Source Control’ menu. 

Merging: Merging swift files is the same as merging any other project. The one aspect of iOS dev git flow you need to be careful about is storyboard merge conflicts. The graphical elements of the storyboard is translated into code which is then 

##### Commonly used shortcuts;
 * cmd + r to build. Faster than physically hunting for the play button each time.
 * Holding down option and clicking any file will open the file in assistant editor window instead of the main editor window
 * control + command + j to jumpt to the definition of a variable. 
 * comand + shift + o: Open quickly. Similar to fuzzy finder in Atom or Sublime. Much faster when you have a lot of files and you know which one you want. 

### Debugging

[Here](https://www.natashatherobot.com/swift-debugging/) is a good tutorial about debugging that will save you much more time than it takes to read. 

Hot top: You can actually add breakpoints to your code AFTER you’ve deployed it to the simulator or to your device. This allows you to print out variables in the console with 'po' without having to put print statements in your code and re-compile. When you’re dealing with 1min+ compile times this will save you hours of dev time. 

For some reason Xcode 8 spits out tons of garbage in the console for no reason. Disable it by following the instructions from the top answer [here](http://stackoverflow.com/questions/37800790/hide-strange-unwanted-xcode-8-logs)

### Misc advice

Be careful when renaming/removing variables that are attached to storyboards via IBOutlets! If the connection is not also removed your app will crash and you will get a very cryptic “key" error that will take you hours to debug if you haven’t seen it before.

Unwrapping options (using !) is very dangerous. I know they’re tempting to use since they cut down on the amount of code you have to write. Unwrap with extreme caution and err on the safe side. On that note, learn when to use the ‘guard’ statement to unwrap as it can provide considerable code cleanliness improvements over ‘if let’.  

## Code

#### Segues

Segues are much, much cleaner if defined pragmatically. 

I personally don’t the PrepareForSegue paradigm. In my opinion is breaks the flow of the code by moving your segue logic to another location in the file. I also find the arrows generated by segues defined in the story board to generate a lot of clutter with more complicated applications. You can organize your storyboard in a logical way that makes the segues obvious. 

Here’s the flow I would recommend. 
```swift
//I would recommend making this a static variable so you have access to it in all files across the lifecycle of the app for all segues
let mainSB = UIStoryboard(name: "Main", bundle: nil)

//make sure to set the identifier in storyboard
if let destinationVC = mainStoryboard.instantiateViewController(withIdentifier: “destination”) as MyViewController {
  //pass through the information 
  destinationVC.someInteger = 5
  destinationVC.someString = “derp”

  //make the segue
  self.present(destinationVC, animated: true, completion: nil)
  //or use "myNavController.pushViewController(destinationVC, animated: true)” when you’re in a navigation stack 
}
```

