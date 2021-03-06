# get-audience-feedback

This demo shows how to capture audience feedback using a QR code and process the request using Node-RED. A random message is  created acknowledging the feedback. The message then is returned to the invoker and also sent as new tweet to Twitter.

![a sample QR Code](/screenshots/qrcode-for-url.png)

## Prerequisites
As this demo creates new tweets on [Twitter](http://www.twitter.com), a Twitter account is needed.

## a) Steps to deploy Node-RED on Bluemix

**Step a1:** If you don't already have a Bluemix account, go to [http://www.bluemix.net] (http://www.bluemix.net) and sign up (it's free).

**Step a2:** Log into your bluemix account.

**Step a3:** Navigate to the Bluemix catalog.

**Step a4:** Click on the "Node-RED Starter" tile. It's in the "Boilerplate" section towards the top of the catalog.

**Step a5:** Enter a unique name for your application into the "Name:" field on the right hand side.

**Step a6:** Click "create" to deploy the application on Bluemix.

**Step a7:** After a minute or two, you should see a notice that your application is now running. Click on the blue link at the top, it should be named something like: http://TheNameThatYouChoseInStepA5.mybluemix.net .

**Step a8:** The above should lead you to a page with the title "Node-RED in Bluemi"x. It has a big red button "Go to your Node-RED flow editor" on the right. Click on it.

**Step 9:** You are now be in your Node-RED flow editor.

## b) Steps to import the Node-RED flow
**Step b1:** Here is the flow that will process the audience feedback. Select all of the JSON below and copy it into your clipboard.

```
[{"id":"a26ac3f9.1d12e8","type":"http in","name":"positive feedback","url":"/yup","method":"get","swaggerDoc":"","x":308,"y":291,"z":"710b37a5.df17d8","wires":[["d1fa4292.7da448"]]},{"id":"37ea13df.1edf7c","type":"http response","name":"return web answer","x":843,"y":249,"z":"710b37a5.df17d8","wires":[]},{"id":"78a362ca.e5ebc4","type":"twitter out","twitter":"","name":"Tweet","x":804,"y":361,"z":"710b37a5.df17d8","wires":[]},{"id":"d1fa4292.7da448","type":"function","name":"positive feedback","func":"var conference=\"#yourConferenceName\";\nvar tstamp=(new Date).toISOString().replace(/t/gi,' ').trim();\n\nvar messages = [\n\t\"great session\",\n\t\"auf den Punkt\",\n\t\"viel gelernt\",\n\t\"great & interactive demo\",\n\t\"guter Vortrag, jetzt ein #Bier\"\n];\n\t\nvar hashtags1 = [\n\t\"#ibmbluemix\",\n\t\"#softlayer\",\n\t\"#paas\",\n\t\"#bluemix\",\n\t\"#ibmcloud\",\n\t\"#cloud\"\n];\n\t\nvar hashtags2 = [\n\t\"#twitter\",\n\t\"#integration\",\n\t\"#nodered\",\n\t\"#demo\",\n\t\"#iot\"\n];\n\t\nvar message = messages[Math.floor(Math.random() * messages.length)];\nvar hashtag1 = hashtags1[Math.floor(Math.random() * hashtags1.length)];\nvar hashtag2 = hashtags2[Math.floor(Math.random() * hashtags2.length)];\n\t\nmsg.payload=\"@data_henrik received positive feedback at \"\n +conference+\" '\"+message+\"' \"+hashtag1+\" \"+hashtag2+\" \"+tstamp;\n\nreturn msg;","outputs":1,"noerr":0,"x":545,"y":291,"z":"710b37a5.df17d8","wires":[["37ea13df.1edf7c","78a362ca.e5ebc4","f652c2cf.7788d"]]},{"id":"e6b60a72.43932","type":"http in","name":"mixed feedback","url":"/boo","method":"get","swaggerDoc":"","x":323,"y":450,"z":"710b37a5.df17d8","wires":[["2d2578a5.231478"]]},{"id":"2d2578a5.231478","type":"function","name":"mixed feedback","func":"var conference=\"#coffeeWorld\";\nvar tstamp=(new Date).toISOString().replace(/t/gi,' ').trim();\n\nvar messages = [\n\t\"an OK session\",\n\t\"selbst meine Oma weiß mehr über die Cloud\",\n\t\"not what I expected\",\n\t\"well, COULD have been a good presentations\",\n\t\"#Bier vorher und Vortrag wäre gut geworden\"\n];\n\t\nvar hashtags1 = [\n\t\"#ibmbluemix\",\n\t\"#softlayer\",\n\t\"#paas\",\n\t\"#bluemix\",\n\t\"#ibmcloud\",\n\t\"#ibm\",\n\t\"#cloud\"\n];\n\t\nvar hashtags2 = [\n\t\"#demo\",\n\t\"#integration\",\n\t\"#nodered\",\n\t\"#enterprise\",\n\t\"#demo\",\n\t\"#iot\"\n];\n\t\nvar message = messages[Math.floor(Math.random() * messages.length)];\nvar hashtag1 = hashtags1[Math.floor(Math.random() * hashtags1.length)];\nvar hashtag2 = hashtags2[Math.floor(Math.random() * hashtags2.length)];\n\t\nmsg.payload=\"@yourTwitterID received mixed feedback at \"\n +conference+\" '\"+message+\"' \"+hashtag1+\" \"+hashtag2+\" \"+tstamp;\n\nreturn msg;","outputs":1,"noerr":0,"x":564,"y":447,"z":"710b37a5.df17d8","wires":[["78a362ca.e5ebc4","af68311.c747a5","45946be9.9775dc"]]},{"id":"f652c2cf.7788d","type":"debug","name":"Debugging","active":false,"console":"false","complete":"payload","x":819,"y":194,"z":"710b37a5.df17d8","wires":[]},{"id":"af68311.c747a5","type":"http response","name":"return web answer","x":840,"y":474,"z":"710b37a5.df17d8","wires":[]},{"id":"45946be9.9775dc","type":"debug","name":"Debugging","active":false,"console":"false","complete":"payload","x":815,"y":526,"z":"710b37a5.df17d8","wires":[]}]
```
**Step b2:** Next import the flow into the Node-RED instance on Bluemix.
![Importing from clipboard](/screenshots/importFromClipboard.png)

- Click on the menu on the upper right of the Node-RED user interface

- Navigate to "Import" and then "Clipboard"

- Paste the content of your clipboard (which should contain the flow that you copied in step 10 above) into the window and click on "Ok"

**Step b3:** You should now see the imported flow in your Node-RED editor.
![Imported flow from clipboard](/screenshots/feedbackFlow.png)

## c) Steps to configure and deploy your Node-RED flow
Now that we have the flow in the Node-RED editor, the next steps involve configuring and adapting the nodes to your likings and your Twitter username.

**Step c1:** Configure the http inbound nodes in your Node-RED editor.
- Doubleclick on the "positive feedback" http node (located on the upper left of the flow). It should bring up the following dialog:

![Edit the httpin node](/screenshots/edit-httpin-node.png)

- The URL determines under which name the positive feedback can be left. Leave it as "yup" or adapt it your likings, e.g., "positive" or "great".

- Repeat the configuration for the "negative feedback" node. The copied flow had the URL set to "boo".

**Step c2:** Configure the function nodes in your Node-RED editor.
- Doubleclick on the "positive feedback" function node (located in the upper middle of the flow). It should bring up a dialog like this:

![Imported flow from clipboard](/screenshots/edit-function-node.png)

- Change the value for the variable "conference" from "#yourConferenceName" to an event name or hashtag your would like to use.
- The result message (returned to the caller and posted to Twitter) uses a random feedback string and two random hashtags. All of them can be modified by adapting the contents of the arrays "messages", "hashtags1" and "hashtags2".
- Towards the bottom of the script the result message is composed. Replace "yourTwitterID" by your Twitter username. Feel free to modify the structure of the message, to add or remove components. Note that the entire message needs to fit within the 140 character limit and that tweets need to have unique content.

**Step c3:** Configure the Twitter output node in your Node-RED editor.
- Doubleclick on the "Tweet" node. If no Twitter configuration node is present, the dialog will look like shown:

![Imported flow from clipboard](/screenshots/add-new-twitter-credentials.png)

 - Click on the icon next to "Add new twitter-credentials". This will bring up a new dialog where you are asked to authenticate with Twitter. Your flow with the "Tweet" node and Twitter are going to exchange authentication tokens that are stored in a (usually invisible) Twitter configuration node within the flow. That token will later allow the "Tweet" node to access Twitter and post messages on your behalf without any further password prompts. Finish the configuration dialogs until you are back to the plain flow.
 -  After all nodes have been configured, the flow can be deployed, i.e., made active. For this click the "Deploy" button. It can be found on the upper left. After clicking the button you should receive a notification that everything has been deployed successfully.
 -  Want to test it? When visiting http://TheNameThatYouChoseInStepA5.mybluemix.net/yourURLfromC1 you should see a response composed of the message parts configured in step c2. In addition check the Twitter timeline for the user you picked in step c3.

## d) Steps to generate QR Code and set up feedback HTML page
**Step d1:** Generate [QR codes](https://en.wikipedia.org/wiki/QR_code) for both the URLs that you configured in step c1. 
- In your QR code generator (if you don't have or know any, [Google is your friend](https://www.google.de/?q=qr+code+generator)) choose the type "URL" and type or paste in your URL for positive feedback. The URL should be something like http://TheNameThatYouChoseInStepA5.mybluemix.net/ThePathPickedInStepC1.
- The generator now should produce an image with a QR code. Depending on the generator you may be able to change the image size or other features. Save the image to a local directory and name the file, e.g., "QRpositive.jpg".
- Repeat the QR code generation for the URL for mixed feedback and save the image as well.

**Step d2:** Save the file "showcase.html" to the same local directory

**Step d3:** Adapt the file "showcase.html" to your QR codes and to your Twitter username.

**Step d4:** Test the feedback demo

# Troubleshooting & Contacts
tbd

# License

See [LICENSE](LICENSE) file.
