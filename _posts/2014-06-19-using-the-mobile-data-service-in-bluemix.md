---
id: 777
title: Using The Mobile Data Service In Bluemix
date: 2014-06-19T13:08:34+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=777
permalink: /2014/06/19/using-the-mobile-data-service-in-bluemix/
dsq_thread_id:
  - 4535001829
categories:
  - BlueMix
  - Cloud
  - PaaS
---
I have stated to look at the mobile services that are within Bluemix and experiment with how developers can use them in their mobile apps.  I decided to start with the Mobile Data service.  This service provides SDKs for iOS and Android (as well as ones for Node.js and client-side Javascript) which allow you to perform basic CRUD operations from your mobile app.  However before I got started playing with the Mobile Data service I needed a mobile app to experiment with.  Since I am an Apple user and I have iOS devices I decided to stick with iOS.  I found a really good <a href="https://developer.apple.com/library/iOS/referencelibrary/GettingStarted/RoadMapiOS/index.html#//apple_ref/doc/uid/TP40011343-CH2-SW1" target="_blank">tutorial</a> from Apple which takes the novice iOS developer through building a ToDo app.  After I finished following the tutorial I was left with a working ToDo app, but the ToDos were not persisted so if I close the application all my ToDos were lost.  This seemed like a perfect app to add the mobile data service to in order to persist the ToDos the user creates.  The steps below assume you have finished following the tutorial from Apple and have a working ToDo app.

The first thing you need to do to get started with the mobile data service is go to the Bluemix catalog and create a new application from the Mobile Cloud boilerplate.

[<img class="alignnone size-full wp-image-778" src="http://ryanjbaxter.com/wp-content/uploads/2014/06/Screen-Shot-2014-06-13-at-9.54.19-AM.png" alt="Screen Shot 2014-06-13 at 9.54.19 AM" width="544" height="232" />](http://ryanjbaxter.com/wp-content/uploads/2014/06/Screen-Shot-2014-06-13-at-9.54.19-AM.png)

After giving your application a name, Bluemix will provision a new application with a number of services, one of them being the Mobile Data service.  (We won&#8217;t use the others in this article, I just use the boilerplate to make it easy to get started.)  After the application is created select the new application from the dashboard and you will be brought to a somewhat familiar application UI.  However the application UI for mobile cloud applications is a little different, notice the application ID.  As we will see later on, this application ID will be important when using the SDK.  In addition there is also a link right next to the application ID to download the SDKs.  Perfect!!

[<img class="alignnone size-full wp-image-779" src="http://ryanjbaxter.com/wp-content/uploads/2014/06/Screen-Shot-2014-06-13-at-9.58.13-AM.png" alt="Screen Shot 2014-06-13 at 9.58.13 AM" width="1024" height="190" />](http://ryanjbaxter.com/wp-content/uploads/2014/06/Screen-Shot-2014-06-13-at-9.58.13-AM.png)

Click the Download SDKs link to download the iOS SDK and extract it from the zip file.  Next you need to add the SDK to the frameworks for your ToDo project in XCode.  To do this drag and drop the IBMBaaS.framework and IBMData.framework files into the frameworks folder in XCode.

[<img class="alignnone size-full wp-image-780" src="http://ryanjbaxter.com/wp-content/uploads/2014/06/Screen-Shot-2014-06-13-at-10.39.54-AM.png" alt="Screen Shot 2014-06-13 at 10.39.54 AM" width="398" height="260" />](http://ryanjbaxter.com/wp-content/uploads/2014/06/Screen-Shot-2014-06-13-at-10.39.54-AM.png)

Once you drag and drop the files you will see a dialog containing options for adding the files, these are the options I chose.

[<img class="alignnone size-full wp-image-782" src="http://ryanjbaxter.com/wp-content/uploads/2014/06/Screen-Shot-2014-06-17-at-2.25.58-PM.png" alt="Screen Shot 2014-06-17 at 2.25.58 PM" width="731" height="492" />](http://ryanjbaxter.com/wp-content/uploads/2014/06/Screen-Shot-2014-06-17-at-2.25.58-PM.png)

Next our native application will need to know what our mobile cloud application ID is in order to use the SDK.  It is best to externalize this into a separate file, a .plist file to be exact.  Right click on the supporting files folder in the project navigator in XCode and select New File from the context menu.  In the iOS section of the New File dialog choose Resource and then choose Property List.  Give the file the name configuration.

[<img class="alignnone size-full wp-image-783" src="http://ryanjbaxter.com/wp-content/uploads/2014/06/Screen-Shot-2014-06-17-at-2.32.08-PM.png" alt="Screen Shot 2014-06-17 at 2.32.08 PM" width="722" height="487" srcset="http://ryanjbaxter.com/wp-content/uploads/2014/06/Screen-Shot-2014-06-17-at-2.32.08-PM-300x202.png 300w, http://ryanjbaxter.com/wp-content/uploads/2014/06/Screen-Shot-2014-06-17-at-2.32.08-PM.png 722w" sizes="(max-width: 722px) 100vw, 722px" />](http://ryanjbaxter.com/wp-content/uploads/2014/06/Screen-Shot-2014-06-17-at-2.32.08-PM.png)

[<img class="alignnone size-full wp-image-784" src="http://ryanjbaxter.com/wp-content/uploads/2014/06/Screen-Shot-2014-06-17-at-2.32.22-PM.png" alt="Screen Shot 2014-06-17 at 2.32.22 PM" width="423" height="329" srcset="http://ryanjbaxter.com/wp-content/uploads/2014/06/Screen-Shot-2014-06-17-at-2.32.22-PM-300x233.png 300w, http://ryanjbaxter.com/wp-content/uploads/2014/06/Screen-Shot-2014-06-17-at-2.32.22-PM.png 423w" sizes="(max-width: 423px) 100vw, 423px" />](http://ryanjbaxter.com/wp-content/uploads/2014/06/Screen-Shot-2014-06-17-at-2.32.22-PM.png)

Select the new configuration.plist file in your supporting files directory and add a new property called applicationId.  Its value should be a String and the value should be the application ID from the mobile cloud application you created in Bluemix.

[<img class="alignnone size-full wp-image-785" src="http://ryanjbaxter.com/wp-content/uploads/2014/06/Screen-Shot-2014-06-17-at-2.34.05-PM.png" alt="Screen Shot 2014-06-17 at 2.34.05 PM" width="407" height="39" />](http://ryanjbaxter.com/wp-content/uploads/2014/06/Screen-Shot-2014-06-17-at-2.34.05-PM.png)

Now we need to read in the new plist file in our code, extract the application ID from it, and initialize the iOS Mobile SDK with it.  This code can live in the AppDelegate.m file within the didFinishLaunchingWithOptions:launchOptions method.  First we need to add some imports to the file in order to get things to compile.  Add the following imports after the existing imports.

<pre>#import &lt;IBMBaaS/IBMBaaS.h>;
#import &lt;IBMData/IBMData.h>;
</pre>

Here is the implementation of didFinishLaunchingWithOptions:launchOptions which reads the plist file and initializes the SDK.

<pre>- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{

    NSString *applicationId = nil;
    
    BOOL hasValidConfiguration = YES;
    NSString *errorMessage = @"";
    
    // Read the applicationId from the configuration.plist.
    NSString *configurationPath = [[NSBundle mainBundle] pathForResource:@"configuration" ofType:@"plist"];
    if(configurationPath){
        NSDictionary *configuration = [[NSDictionary alloc] initWithContentsOfFile:configurationPath];
        applicationId = [configuration objectForKey:@"applicationId"];
        if(!applicationId || [applicationId isEqualToString:@""]){
            hasValidConfiguration = NO;
            errorMessage = @"Open the configuration.plist and set the applicationId to the BlueMix applicationId";
        }
    }
    
    if(hasValidConfiguration){
        // Initialize the SDK and BlueMix services
        [IBMBaaS initializeSDK: applicationId];
        [IBMData initializeService];
    }else{
        [NSException raise:@"InvalidApplicationConfiguration" format: @"%@", errorMessage];
    }
    return YES;
}
</pre>

Now we need to make some changes to our model object, ToDoItem, to make it so the IBM Mobile Data SDK can transform it into JSON and persist it to the DB.   Open ToDoItem.h.  The ToDoItem model currently extends NSObject, but we need to change it so it extends IBMDataObject from the SDK.  In addition, we need to add the modifiers nonatomic and copy to the properties of the interface.  Here is what my ToDoItem.h file looked like after doing this.

<pre>#import &lt;Foundation/Foundation.h&gt;
#import &lt;IBMData/IBMData.h&gt;

@interface IBMToDoItem : IBMDataObject
@property (nonatomic, copy) NSString *itemName;
@property (nonatomic, copy) NSNumber *completed;

@end
</pre>

You will notice that I took out the creationDate property from the Apple tutorial.  The tutorial said to add this but by the end  of the tutorial I did not see it being used anywhere so I removed it.  I also changed the type of completed to NSNumber.  The IBM Mobile Data SDK seemed to have a problem converting non-pointers to JSON (probbaly something here I am not understanding) so I changed the type to NSNumber and made it a pointer.

We also need to make a few changes in the ToDoItem implementation file, open ToDoItem.m.  We need to add @dynamic to completed and itemName to provide getters and setters for the Mobile Data SDK to use.  We also need to implement the method dataClassName which provides the object name that the objects will be persisted under in the mobile backend.  In addition we need to override the initialize method so we can call the registerSpecialization method from the super class.  This creates a mapping between the Objective-c class and the JSON object.  Here is what my ToDoItem.m file looks like after doing this.

<pre>#import "IBMToDoItem.h"

@implementation IBMToDoItem

@dynamic itemName;
@dynamic completed;

+(void) initialize
{
    [self registerSpecialization];
}

+(NSString*) dataClassName
{
    return @"ToDoItem";
}

@end
</pre>

Most of the work to manipulate the data in the Mobile Data service on Bluemix happens in the ToDoListTableViewController.m file.  First lets tackle storing new ToDos to the backend.  When a new ToDo is added in the UI the unwindToList:seague method is called, so this is where we want to persist our ToDoItem object to the Mobile Data backend.  This can be done by calling the saveInBackgroundWithCallback method in our ToDoItem.  This method is coming from the IBMDataObject class which our ToDoItem extends.  Here is what my unwindToList method looks like.

<pre>- (IBAction)unwindToList:(UIStoryboardSegue *)seague
{
    IBMAddToDoItenViewController *source = [seague sourceViewController];
    IBMToDoItem *item = source.toDoItem;
    if (item != nil) {
        [self.toDoItems addObject:item];
        [self.tableView reloadData];
        [item saveInBackgroundWithCallback:^(NSError *error, IBMToDoItem *addedItem) {
            if(error){
                // Error handing code here
                NSLog(@"createItem failed with error: %@", error);
            }
        }];
    }
    
}
</pre>

After we create new items and persist them to the Mobile Data service we need to retrieve them when the app loads and display them in the list.  We can do this in the loadInitialData method by using the query class-level method on the ToDoItem class.  Here is what my loadInitialData method looks like.

<pre>- (void)loadInitialData {
    IBMQuery *qry = [IBMToDoItem query];
    [qry findObjectsInBackgroundWithBlock:^(NSError *error, NSArray *objects){
        if (!error) {
            self.toDoItems = [NSMutableArray arrayWithArray: objects];
            [self.toDoItems sortUsingComparator:^NSComparisonResult(IBMToDoItem* item1, IBMToDoItem* item2) {
                return [item1.itemName caseInsensitiveCompare:item2.itemName];
            }];
            
            [self.tableView performSelectorOnMainThread:@selector(reloadData) withObject:nil waitUntilDone:NO];
        }else{
            // Error handing code here
            NSLog(@"listItems failed with error: %@", error);
        }
    }];
}
</pre>

Next we want to address the update case, so when you make a ToDo as done that change is updated in the Mobile Data backend.  This happens in the didSelectRowAtIndexPath:indexPath method.  When an IBMDataObject is updated we can use the saveInBackgroundWithCallback method to save the updated object to the DB.  With some small modifications to the existing method code we can save updates.  Notice in the code below that we also make the necessary changes to deal with the new type of the completed property on the ToDoItem.

<pre>- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath
{
    [tableView deselectRowAtIndexPath:indexPath animated:NO];
    IBMToDoItem *tappedItem = [self.toDoItems objectAtIndex:indexPath.row];
    tappedItem.completed = [NSNumber numberWithBool:![tappedItem.completed boolValue]];
    [tableView reloadRowsAtIndexPaths:@[indexPath] withRowAnimation:UITableViewRowAnimationNone];
    [tappedItem saveInBackgroundWithCallback:^(NSError *error, IBMToDoItem *updatedItem) {
        if(error){
            // Error handing code here
            NSLog(@"updateItem failed with error: %@", error);
        }
    }];
}
</pre>

Because the completed property of the ToDoItem is no longer a boolean we also need to make a small change in the cellForRowAtIndexPath:indexPath method which adds and removes the check box in the UI.  Right now the if statement which checks the value of the completed property will always return true no matter what the number value is.  To fix this we just need to translate the NSNumber to a boolean.  Here is the updated method.

<pre>- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"ListPrototypeCell" forIndexPath:indexPath];
    
    IBMToDoItem *toDoItem = [self.toDoItems objectAtIndex:indexPath.row];
    cell.textLabel.text = toDoItem.itemName;
    if ([toDoItem.completed boolValue]) {
        cell.accessoryType = UITableViewCellAccessoryCheckmark;
    } else {
        cell.accessoryType = UITableViewCellAccessoryNone;
    }
    
    return cell;
}
</pre>

That is it as far as coding is concerned so lets make sure our code compiles.  If you do a build by going to Product -> Build you will see the build fail with a bunch of build errors.  This is because we need to add some linker flags.  To do this, click on the ToDo List project in the project navigator and then select the ToDoList target.  Next click the Build Settings tab and make sure All is selected.  Under the Linking section select Other Linker Flags.  You will need to add the -ObjC and -lsqlite3 linker flags.  Here is a screenshot which describes this a little better.

[<img class="alignnone size-full wp-image-790" src="http://ryanjbaxter.com/wp-content/uploads/2014/06/Screen-Shot-2014-06-17-at-5.11.26-PM.png" alt="Screen Shot 2014-06-17 at 5.11.26 PM" width="1422" height="716" srcset="http://ryanjbaxter.com/wp-content/uploads/2014/06/Screen-Shot-2014-06-17-at-5.11.26-PM-300x151.png 300w, http://ryanjbaxter.com/wp-content/uploads/2014/06/Screen-Shot-2014-06-17-at-5.11.26-PM-1024x515.png 1024w, http://ryanjbaxter.com/wp-content/uploads/2014/06/Screen-Shot-2014-06-17-at-5.11.26-PM.png 1422w" sizes="(max-width: 1422px) 100vw, 1422px" />](http://ryanjbaxter.com/wp-content/uploads/2014/06/Screen-Shot-2014-06-17-at-5.11.26-PM.png) Now go to Product -> Build and your project should now build successfully.

Now that our code compiles we can try running the application in the iOS emulator, click the Play button in the upper left hand corner of XCode to launch the emulator.  As you add  and update new ToDos they should be persisted to the mobile data backend.  You can verify this by going to your application in Bluemix and clicking the Mobile Data service on the left.

[<img class="alignnone size-full wp-image-791" src="http://ryanjbaxter.com/wp-content/uploads/2014/06/Screen-Shot-2014-06-17-at-9.37.30-PM.png" alt="Screen Shot 2014-06-17 at 9.37.30 PM" width="562" height="692" />](http://ryanjbaxter.com/wp-content/uploads/2014/06/Screen-Shot-2014-06-17-at-9.37.30-PM.png)

In the Manage Data section of the console you can browse the data stored in the DB from your application.

[<img class="alignnone size-full wp-image-793" src="http://ryanjbaxter.com/wp-content/uploads/2014/06/Screen-Shot-2014-06-17-at-9.39.51-PM.png" alt="Screen Shot 2014-06-17 at 9.39.51 PM" width="2626" height="1548" srcset="http://ryanjbaxter.com/wp-content/uploads/2014/06/Screen-Shot-2014-06-17-at-9.39.51-PM-1024x603.png 1024w, http://ryanjbaxter.com/wp-content/uploads/2014/06/Screen-Shot-2014-06-17-at-9.39.51-PM.png 2626w" sizes="(max-width: 2626px) 100vw, 2626px" />](http://ryanjbaxter.com/wp-content/uploads/2014/06/Screen-Shot-2014-06-17-at-9.39.51-PM.png)

I hope this gives you a good idea on how you can use the Mobile Data service in Bluemix to persist data for your native mobile applications.  There are many more features of the service that I did not mention in this post, but you can learn about them by reading through the <a href="https://www.ng.bluemix.net/docs/Services/MobileData/index.jsp" target="_blank">documentation</a> for the service.