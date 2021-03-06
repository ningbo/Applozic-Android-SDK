Refer to the below documentation for a deeper integration if you wish to perform chat operation directly from your app's interface without using the Applozic UI toolkit:


####Account registration   
   
Import
```
import com.applozic.mobicomkit.api.account.register.RegisterUserClientService;      
```

Code
```
new RegisterUserClientService(activity).createAccount
(USER_EMAIL, USER_ID, USER_PHONE_NUMBER, GCM_REGISTRATION_ID);         
 ``` 

####Send message   

Import
```
import com.applozic.mobicomkit.api.conversation.MobiComConversationService;         
```

Code
```
 public void sendMessage(Message message)        
 {             
   ...        
 }                
```

Example
```
new MobiComConversationService(activity).sendMessage(new     
Message("contact@applozic.com", "hello test"));         
```


####Message list      

Import
```
import com.applozic.mobicomkit.api.conversation.MobiComConversationService;
```

  
i) Get single latest message from each conversation    

Code
```
 public synchronized List<Message> getLatestMessagesGroupByPeople()        
 {            
  ...         
 }                              
```


ii) Get messages of logged in user with another user by passing userId, startTime and endTime. 
startTime and endTime are considered in time in milliseconds from 1970.       

Code
```
 public List<Message> getMessages(String userId, Long startTime, Long endTime)        
 {            
  ...           
 }                           
```

####Custom Message
Send an automated custom message to connect 2 users. The message layout will be same for both users so that it doesn't look like a sent message.


Code
```
Message message = new Message("contact@applozic.com", "hey! here's a match <3");
new MobiComMessageService(this, MessageIntentService.class).sendCustomMessage(message);
```

Customize the background color for the custom message:
Code
```
ApplozicSetting.getInstance(this).setColor(ApplozicSetting.CUSTOM_MESSAGE_BACKGROUND_COLOR, Color.parseColor("#FFB3E5FC"));
```



###  Contacts           

Note:This methods are for creating local conatcts and only stored locally.

***Creating Contact list***         


You can create the contact list in two easy steps by using AppContactService.java api. 
Sample method **buildContactData()** for adding contacts is present in sample app MainActivity.java.

####Step 1: Creating contact   

Create
```
    Contact contact = new Contact();            
    contact.setUserId(<userId>);           
    (Unique ID to identify contact )                 
    contact.setFullName(<full name of contact>);                 
    contact.setEmailId(<EmailId>);               
    contact.setImageURL(<image http URL OR android resource drawable  >);               
    (in case of drawable use R.drawable.<resource_name>)                         
```

Example :        

```
    Contact contact = new Contact();                 
    contact.setUserId("adarshk");           
    contact.setFullName("Adarsh");               
    contact.setImageURL("R.drawable.applozic_ic_contact_picture_holo_light");           
    contact.setEmailId("github@applozic.com");                
```

####Step 2: Save contact

Save the contact using AppContactService.java add() method.
 
```
    Context context = getApplicationContext();           
    AppContactService appContactService = new AppContactService(context);            
    appContactService.add(contact);                           
```



**AppContactService.Java at a glance**



AppContactService.java provides methods you need to add, delete and update contacts.

Add single contact
```
add(Contact contact)
```

Add multiple contacts
```
addAll(List<Contact> contactList)
```

Delete contact
```
deleteContact(Contact contact)
```

Delete contact by Id
```
deleteContactById(String contactId)
```

Read contact by Id
```
getContactById(String contactId )
```

Update contact
```
updateContact(Contact contact)
```

Update or Insert contact
```
upsert(Contact contact)
```



### Group 

Enable Group Messaging for a user
```
ApplozicSetting.getInstance(context).showStartNewGroupButton();
```

Disable Group Messaging for a user
```
ApplozicSetting.getInstance(context).hideStartNewGroupButton();
```


####1) Create Group

Create the Group with Group Name and Group Members. 

Import

```
import com.applozic.mobicomkit.uiwidgets.async.ApplozicChannelCreateTask;
```

Code
 ```
  ApplozicChannelCreateTask.ChannelCreateListener channelCreateListener = new ApplozicChannelCreateTask.ChannelCreateListener() {
            @Override
            public void onSuccess(Channel channel, Context context) {
                Log.i("ApplzoicChannelCreate", "After success full creation of channel");
                Log.i("Channel", "Channel object:" + channel);//channel.getKey() is a channel key or group id
            }

            @Override
            public void onFailure(Exception e, Context context) {

            }
        };

                String groupName = "Applozic Group"; // Name of group.
                List<String> groupMemberList = new ArrayList<String>(); // List Of unique group member Names.
                groupMemberList.add("member1");   // Put userId of the user
                groupMemberList.add("member2");
                groupMemberList.add("member3");
                groupMemberList.add("member4");
                
ApplozicChannelCreateTask applozicChannelCreateTask = new ApplozicChannelCreateTask(context, channelCreateListener, groupName, groupMemberList);
applozicChannelCreateTask.execute((Void) null);

 ```

####2) Add Member to Group
  
Import
```
  import com.applozic.mobicomkit.uiwidgets.async.ApplozicChannelAddMemberTask;
```

Code
 ``` 
 ApplozicChannelAddMemberTask.ChannelAddMemberListener channelAddMemberListener =  new ApplozicChannelAddMemberTask.ChannelAddMemberListener() {
            @Override
            public void onSuccess(String response, Context context) {
                Log.i("ApplozicChannelMember","Add Response:"+response);

            }

            @Override
            public void onFailure(String response, Exception e, Context context) {

            }
        };

        ApplozicChannelAddMemberTask applozicChannelAddMemberTask =  new ApplozicChannelAddMemberTask(context,channelKey,userId,channelAddMemberListener);//pass channel key and userId whom u want to add to channel
        applozicChannelAddMemberTask.execute((Void)null);

 ```
   
  __Parameters:__

  __channelkey__: Is a unique Integer type to which group/channel u want to add a member to it
  
  __userId__:Unique userId of string type to whom u want to add to group/channel
  
   __Return response__: If user added successfully in group/channel it returns success else error 
 
 
####3) Remove Member from Group
 
Import  
```
import com.applozic.mobicomkit.uiwidgets.async.ApplozicChannelRemoveMemberTask;
```

Code
  ```
  ApplozicChannelRemoveMemberTask.ChannelRemoveMemberListener channelRemoveMemberListener = new ApplozicChannelRemoveMemberTask.ChannelRemoveMemberListener() {
            @Override
            public void onSuccess(String response, Context context) {
                Log.i("ApplozicChannel","remove member response:"+response);//after removing a member from channel the call back will come here

            }

            @Override
            public void onFailure(String response, Exception e, Context context) {

            }
        };

        ApplozicChannelRemoveMemberTask applozicChannelRemoveMemberTask =  new ApplozicChannelRemoveMemberTask(context,channelKey,userId,channelRemoveMemberListener);//pass channelKey and userId whom you want to remove from channel
        applozicChannelRemoveMemberTask.execute((Void)null);
 ```
  
  __Parameters:__
  
 __channelKey__:Is a Unique Integer type from which u want to remove member
 
 __userId__:Unique userId of string type whome u want to remove from group/channel

 __Return response__: If user Removed successfully from group/channel it returns success else error 
 
 
  __NOTE:__ Only admin can remove member from the group/channel.
  
 
####4) Leave Group
 
Import
```
import com.applozic.mobicomkit.uiwidgets.async.ApplozicChannelLeaveMember;
```
  
Code
  ```
  ApplozicChannelLeaveMember.ChannelLeaveMemberListener  channelLeaveMemberListener  = new ApplozicChannelLeaveMember.ChannelLeaveMemberListener() {
            @Override
            public void onSuccess(String response, Context context) {
                Log.i("ApplozicChannel","Leave member respone:"+response);//After leaving  member from channel  call back will come here
            }

            @Override
            public void onFailure(String response, Exception e, Context context) {

            }
        };

        ApplozicChannelLeaveMember applozicChannelLeaveMember = new ApplozicChannelLeaveMember(context,channelKey,userId,channelLeaveMemberListener);//pass channelKey and userId
        applozicChannelLeaveMember.execute((Void)null);
  ```
 
   __Parameters:__
 
 __channelKey__:Unique Integer type 
 
 __userId__ :Unique userId of string type
 
 __Return response__: success or error 
 
 Note:This is only for logged in user who want's to leave from group
 
####5) Change Group Name

Import

```
  
import com.applozic.mobicomkit.uiwidgets.async.ApplozicChannelNameUpdateTask;
```
  
Code

```
  ApplozicChannelNameUpdateTask.ChannelNameUpdateListener channelNameUpdateListener = new ApplozicChannelNameUpdateTask.ChannelNameUpdateListener() {
            @Override
            public void onSuccess(String response, Context context) {
                Log.i("ApplozicChannel", "Name update:" + response);//after updating channel name call back will come here
            }
            @Override
            public void onFailure(String response, Exception e, Context context) {

            }
        };

        ApplozicChannelNameUpdateTask channelNameUpdateTask = new ApplozicChannelNameUpdateTask(context, channelKey, channelName, channelNameUpdateListener);//pass context ,channelKey,chnanel new name 
        channelNameUpdateTask.execute((Void) null);

 ```
 __Parameters:__
 
 __channelKey__:Unique Integer type 
 
 __channelName__ :new Channel name you want to change
 
  __Return response__: If group/channel name successfully changed it returns success else error 
  
  
  
  ####Open Activity on tap of toolbar in Chat Screen

  Add the following in your androidmanifest.xml
  
  Code
  ```
  <meta-data
             android:name="com.applozic.mobicomkit.uiwidgets.toolbar.tap.activity"
             android:value="PUT_ACTIVITY_CLASS_HERE" />
 ```

  This activity will receive the “userId” of the selected chat in intent.
  
