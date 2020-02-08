
# Build a Slack Clone with React and Pusher Chatkit

[github-star-badge]: https://img.shields.io/github/stars/pusher/build-a-slack-clone-with-react-and-pusher-chatkit.svg?style=social
[github-star]: https://github.com/pusher/build-a-slack-clone-with-react-and-pusher-chatkit/stargazers

Built a chat app with React and [Chatkit](https://pusher.com/chatkit?utm_source=github&utm_campaign=build-a-slack-clone-with-react-and-pusher-chatkit).

## What is Chatkit?

[Chatkit](https://pusher.com/chatkit?utm_source=github&utm_campaign=build-a-slack-clone-with-react-and-pusher-chatkit) is a hosted API that helps you build impressive chat features into your applications with less code. Features like,

* Group chat
* One-to-one chat
* Private chat
* Typing indicators
* "Who's online" presence
* Read receipts
* Photo, video, and audio messages


To get started, download the starter template then run `npm install`:

```
git clone https://github.com/karshd3v/slack-clone-chatkit.git
cd slack-clone-chatkit
npm install
```

## Step 2. Create your own Chatkit instance

Now you've downloaded the starter template, let's create a Chatkit instance.


Give your instance any name (I called mine "React Chat Tutorial") then take note of your **Instance Locator** and **Secret Key** in the **Keys** tab. We'll need them both in the next section.



## Step 3. Setup a basic Node server

While most interactions will happen on the client, Chatkit also needs a server counterpart to create and manage users securely:

```
npm install --save @pusher/chatkit-server
```

Then update `./server.js`:

```diff

const chatkit = new Chatkit.default({
+  instanceLocator: 'YOUR INSTANCE LOCATOR',
+  key: 'YOUR KEY',
})
```
Remember to replace **"YOUR INSTANCE LOCATOR"** and **"YOUR KEY"** with your own respective values.
Boom 💥! That's all we need to do on the server. Let's move on to the client...

## Step 4. Identifying the user

Run the application using `npm start` and you'll see that the screen is rendered:

![](media/what-is-your-username-component.png)

## Step 5. Connect to your Chatkit instance

Earlier, we installed `@pusher/chatkit-server`. Now we're in client-land, you'll need to install [`@pusher/chatkit-client`](https://www.npmjs.com/package/@pusher/chatkit-client) as well:

```
npm install --save @pusher/chatkit-client
```

Then update `ChatScreen.js`:

```diff

  componentDidMount () {
    const chatManager = new Chatkit.ChatManager({
+      instanceLocator: 'YOUR INSTANCE LOCATOR',
      userId: this.props.currentUsername,
      tokenProvider: new Chatkit.TokenProvider({
        url: 'http://localhost:3001/authenticate',
      }),
    })
```

Remember to replace **"YOUR INSTANCE LOCATOR"** with yours that you noted earlier.

## Step 6. Create a Chatkit room

When using Chatkit, all messages are sent to a Chatkit room.

Rooms can be created programmatically (on the server or client using `createRoom`), or in the dashboard Inspector.

Creating rooms from the Inspector isn't really a good practice (it's mainly intended for testing) but for the purpose of this walkthrough, we'll do it anyway.

In the dashboard, head to the **Console** tab, where you'll find the Inspector and create a user with any name. I will call mine "Admin".

Then, create a room called "General":

![](media/create-room.png)

It is really important to note the unique **Room id** highlighted above.

## Step 7. Subscribe to new messages

Then update `ChatScreen.js`:

```diff

    chatManager
      .connect()
      .then(currentUser => {
        this.setState({ currentUser })
        return currentUser.subscribeToRoom({
+          roomId: "YOUR ROOM ID",
          messageLimit: 100,
          hooks: {
            onMessage: message => {
              this.setState({
                messages: [...this.state.messages, message],
              })
            },
          },
        })
      })

```

Remember to replace **YOUR ROOM ID** with your own room ID that you noted earlier.

## Conclusion

In this walkthrough, you built a complete chat application with

* group chat;
* a “Who’s online” list; and,
* typing indicators

Because we used Chatkit, we also get some bonus features for free:

* message history (refresh the page and you’ll see up to 100 of the most recent messages);
* reliability in the case that the client temporarily loses connection (Chatkit handles disconnects gracefully); and,
* the ability to scale without needing to worry about infrastructure

We wrote a fair amount of code, but none of it was particularly complicated.

Chatkit has a minimal but powerful API that manages all our chat data for us. All we had to do is take that data and render it for the user.

Want to keep building? Why not add rich media support and read receipts? Chatkit supports both:

* [Read receipts](https://docs.pusher.com/chatkit/reference/javascript#cursors)
* [File API](https://docs.pusher.com/chatkit/reference/javascript#attachment)

