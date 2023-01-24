# PennBook
A Facebook-like web application with functionality for post interaction, social networking features, chatrooms, and news feed recommendations. This is our final project for NETS-2120.

**Full Names**:  Ethan Ma, Eric Wang, Kristine Yip, Tin Do

---

## Implemented Features
  * Tin: Friend Visualizer, Friend Notifications
  * Ethan: Chat, Chat Notifications
  * Kristine: Homepages, Friend page
  * Eric: Newsfeed, News Search

**_Features_**:
- User Accounts/Security: users can sign in with a hashed password, and modify all account details including username
- Walls/Homepages: Users can comment, make, and like posts. They can also see the posts and status updates of their friends. Walls are updated asynchronously to keep information recent. Users can send, receieve, and reject friend requests using Linked-in style invitations. 
- User Search: Users can be searched for using a searchbar which utilizes a user prefix table to allow for autocompletion
- Chat: Users can create group chats which allow friends to see who's online. Users can send, recieve and reject chat invitations that connect them to chat rooms. Chat rooms are persistent and a user can be in multiple.
- Visualizer: The friend network can be viewed through the visualizer
- News: Users can change their news interests at any time and search for articles using a search bar. Articles are ordered on term frequency and can be liked or unliked. Articles recieve recommendations informed by the adsorption algorithm in their newsfeed. The algorithm takes into account liked categories, friends, and liked articles. 

_**Extra-Credit Tasks**_
- Infinite Scrolling
- Chat Notifications
- Linkedin Style Friend Requests with Confirmation
- Changing Username (unique userID in place of username to identify users)


## Description of source files

/views:
- contains .ejs files for rendering all pages on the frontend

/routes:
- chatRoute.js, friendRoutes.js, newsRoutes.js, routes.js, visualizerRoutes.js
- contains backend routes for the back-end dependent features

/public/scripts
- contains friendVisualizer.js, getFriendsPosts.js, getPosts.js and homepage.js scripts
- contains javascript code for rendering dynamic content on pages using jQuery/Ajax

/public/stylesheets:
- homePage.css
- contains css for styling frontend components

/newsstuff:
- ComputeRanks.java: code for locally running adsorption
- ComputeRanksLivy.java: code for submitting the adsorption job to Livy
- LoadNetwork.java: code for locally loading the keyword table for news search and the news table for news article details
- SocialRankJob.java: code for running the adsorption algorithm on Livy

/models:
- database.js, friendDatabase.js
- contain javascript code for DynamoDB calls


**Did you personally write _all_ the code you are submitting?**
  [X] Yes
  [ ] No

**Did you copy any code from the Internet, or from classmates?**
  [ ] Yes
  [X] No

**Did you collaborate with anyone on this assignment?**
  [ ] Yes
  [X] No 



## Instruction for Running the Project:

1. Begin by navigating to the NetsFinalProject Folder and calling npm install once inside
2. Initialize All DynamoDB Tables and an EMR cluster, add the livy URI to ComputeRanksLivy

**Here are the initialization details for all necessary tables:**

| Name | Partition key | Sort key |
|-----------|:-----------:|-----------:| 
| Accounts |	userID (S) |	- |
| ArticleWeights |	headline (S) |	weight (N) |
| ChatRoomMembers |	chatID (S) | userID (S) |
| ChatRoomMessages |	chatID (S) |	timestamp (N) |
| ChatRooms |	userID (S) |	chatID (S) |
| Comments |	postID (S) |	commentID (S) |
| Friends |	followerID (S) |	followedID (S) |
| Likes |	postID (S) |	userID (S) |
| News |	headline (S) |	date (S) |
| NewsInterests |	userID (S) |	category (S) |
| NewsLikes |	headline (S) |	userID (S) |
| NewsRec |	articledate (S) |	headline (S) |
| NewsSearch |	keyword (S) |	headline (S) |
| NewsWeights |	userID (S) |	headline (S) |
| Notification |	recipientID (S) |	timestamp (S) |
| Posts1	| userID (S)	| postID (S) |
| Posts2	| creatorID (S) |	postID (S) |
| Prefix	| prefix (S) |	- |
| Usernames	| username (S) |	- |

3. Invoke node app.js to run the application at localhost8080
4. To run adsorption, navigate to the newsstuff folder and invoke mvn install
5. Run adsorption with mvn package followed by mvn exec:java@livy 
6. To load the necessary DynamoDB tables for news, call mvn exec:java@loader 
7. To load the necessary articles, the main function in LoadNetwork may need to be changed to include both runkeywords() and run() as function calls
8. Enjoy!
