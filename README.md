**Coming Soon**:



>”A photo gallery can become a pretty complex component of an app very quickly. The EBPhotoPages project demonstrates how a developer could use the State Pattern to control the behavior of an interface with numerous features. This pattern grants the ability to create new behaviors for a custom implementation of the EBPhotoPages photo gallery without having to modify or understand much of the original source code.”

![Alt text](/1.png "Screenshot")|![Alt text](/2.png "Screenshot")|
![Alt text](/3.png "Screenshot")|![Alt text](/4.png "Screenshot")
![Alt text](/5.png "Screenshot")|![Alt text](/6.png "Screenshot")
![Alt text](/7.png "Screenshot")

About
---------
EBPhotoPages is a photo gallery library for displaying pages of photos and their meta data in a scrollview. Users are able to zoom photos in and out, as well as create, edit or delete comments and tags, share or delete a photo, and report inappropriate photos. All photos and content are loaded asynchronously. User permissions for a photo are controlled through a delegate protocol. No graphic files are required for the gallery as icons are drawn in code.

The library was designed using a state pattern to control the behavior of the gallery, so that other developers can easily modify or add new states without having to understand too much of the original code.

How State Objects work in EBPhotoPages
---------
At any given time, the photo gallery has a state object assigned as it’s current state, when a user event is received, the photo gallery’s state object takes responsibility for how the photo gallery should respond.  

By convention, state objects are decoupled from each other. This means they are completely unaware of other state objects, and they also hold no data of their own. You can freely create and destroy them without fear (or guilt…). 

When a state object decides it’s time to transition to a new state, it simply calls upon a method within the photo gallery that replaces the current state object with a new instance of the next state object. 

To create a new custom State for your implementation of the photo gallery, you should do two things:

1) Create a new state object that is a subclass of the EBPhotoPagesState object, and override the methods you’re interested in. 
(example: EBPhotoPagesCoolNewState)

2) Create a category of the EBPhotoPages that contains the method(s) for your custom transitions. The purpose of a transition method is usually to set the “currentState” object of the EBPhotoPagesController to a new instance of your new custom state. 
 (example: -(void)transitionToCoolNewState;)

3) Decide how the user will reach this new custom state. If there is a special button in the interface that takes the user to this state when tapped, then make sure the EBPhotoPages calls your new transition method for that user event. If you are injecting this custom state somewhere in between the default state machine flow (the normal default behavior of the EBPhotoPagesController), then this may require a bit more overriding of existing classes so that your new state is reached instead of the original ones. 

4) Don’t forget to implement some way for a user to exit your custom state and go back to a default idle browsing state!




Features
---------

+ **Tags**: If you don’t already know, a tag is a point of interest at a certain spot within a photo (usually a person’s face). A tag’s coordinates in a photo are normalized between 0 and 1, so that the point is independent of the actual photo’s current resolution. For usability purposes, tags do not sit exactly on the pixel that was tagged, but rather they have padding that positions them a few pixels below it so the point of interest isn’t overly obscured by the tag. 

+ **Comments**: As you can imagine, a comment is a user submitted text that accompanies a photo. The comment also includes meta data such as the user’s name, their avatar, and time of posting. The order of the comments appearance is determined by the order in which the datasource returns them.

+ **Zero Graphic Files**: Amazingly, the EBPhotoPages library requires no graphic files for its default toolbar icons because everything is drawn natively in code and converted to a UIImage. However, if you want to use actual graphic files for custom icons, you can always provide them directly by returning them through the appropriate methods as well. 


Things to consider when implementing EBPhotoPages
---------
+ _Caching_: The EBPhotoPages do not implement their own caching system for photo content. That responsibility is passed on to whatever you decide to use as the datasource for the EBPhotoPages. You should probably pre-fetch content ahead of time and save it for faster loading if speed is a concern (which is usually the case…). 

+ _User Permission_: When a user takes some action on a photo such as commenting or tagging or deleting (and others), the EBPhotoPages will ask it’s delegate if the user has permission to do that. This approach creates flexibility in controlling what a user can and can’t do on a per-photo basis. This means you should think about how to organize user permissions on your end. 

+ _Creating/Destroying content_:  Prepare your backend for the ability to post new content or delete existing content when the EBPhotoPages notifies your delegate that the user has initiated such an action. It would not be a good idea to let the EBPhotoPages itself be responsible for actual removing or posting the data to your servers directly. 

+ _EBPhotoPagesFactory_: This factory class is the one-stop shop for all UI objects. This class is responsible for instantiaing UI Elements and returning them to the EBPhotoPagesController. If you wish to customize the look of your photo gallery implementation, this is the first place you should check. Some other UI elements are not yet created by this class, but the plan is to eventually move them into it. 


Opportunities for Contribution
---------
Although this library has a lot of features and over 5,000 lines of code, there’s still some challenges to solve. Feel free to improve on the library and submit pull requests. In particular, these areas need some attention:

+ _TagPopovers_: Bless the tag popovers, they do their best to stay within the photo boundaries so they are not cut off screen, but this behavior could still be improved. Right now, the implementation is a bit simplistic, the arrow points need to be drawn closer to where the actual tagged pixel is, not just shoved along. Consideration of a photo’s zoom scale should also be taken into account.

+ _Deleting stuff_: Once upon a time deleting photos worked flawlessly, but somewhere along the way there was an update and something broke and now it’s kind of buggy (though photos do appear to delete). Guess we should have unit tested this after all…

+ _Alternative toolbar layouts_: The current toolbars work fine, but there’s always better or different ways of doing things. If a developer could simply set a toolbar to adopt a different layout style by just setting an enumerated property to a new value that would be sweet. The more variety the better.

+ _iOS 7 features_: This project was started way before iOS 7. Having the option to include some of the physics based features or parallax into the gallery would be awesome. Remember to add a property to toggle these on and off. 

+ _Networking_: The code related to loading content is made to run asynchronously, but it has not been thoroughly tested on a consumer web scale application of any sort. Someone with more experience here may want to try offering some improvements from practical experience.  

+ _Likes_: One of the more noticeable features missing from EBPhotoPages is the ability to “like” photos.

+ _Localization_: Every string used for the interface is ready for localization with NSLocalizedString. Currently, there are no localization files provided with the library so only English is represented. If you want your language for your culture represented, you should consider adding and sharing localization files to the project!

+ _Comment Pagination_: When a photo amasses an staggering number of comments, it would be impractical to make a web request for all of them at once. A system for allowing pagination of comments is needed, as currently the EBPhotoPages does not allow for new comments to be loaded in by user request.


Known Issues
---------
+ sizeWithFont: is deprecated in iOS7, needs to be replaced.
+ The loading indicator has a tendency to not show up sometimes. 
+ Deleting comments doesn’t animate too smoothly. 
+ Creating tags while in landscape mode is a bit messed up.
+ (!) Tags cause the app to crash after you edit or delete them at the moment.



License
--------
_EBPhotoPages_ uses the MIT License:

>Copyright (c) 2013 Eddy Borja

>Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

>The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

>THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

Credits
---------

_EBPhotoPages_ and its components were created by Eddy Borja.


More Stuff
---------
Be sure to check out these other libraries:

[MLPSpotlight](https://github.com/EddyBorja/MLPSpotlight)<br />
[UIColor+MLPFlatColors](https://github.com/EddyBorja/UIColor-MLPFlatColors)<br />
[MLPAccessoryBadge](https://github.com/EddyBorja/MLPAccessoryBadge)<br />
[MLPAutoCompleteTextField](https://github.com/EddyBorja/MLPAutoCompleteTextField)<br />
