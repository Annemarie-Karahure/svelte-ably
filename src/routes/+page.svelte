<script>
const Ably = require('ably/promises');

async function publishSubscribe() {

  // Connect to Ably with your API key
  const ably = new Ably.Realtime.Promise("8Wzsng.VrNkFA:qbezZObr-GSPYTN0uRsS4NuSlAZdLTZRCq2tUVkeReU")
  ably.connection.once("connected", () => {
    console.log("Connected to Ably!")
  })

  // Create a channel called 'get-started' and register a listener to subscribe to all messages with the name 'first'
  const channel = ably.channels.get("get-started")
  await channel.subscribe("first", (message) => {
    console.log("Message received: " + message.data)
  });

  // Publish a message with the name 'first' and the contents 'Here is my first message!'
  await channel.publish("first", "Here is my first message!")

  // Close the connection to Ably after a 5 second delay
  setTimeout(async () => {
    ably.connection.close();
      await ably.connection.once("closed", function () {
        console.log("Closed the connection to Ably.")
      });
  }, 5000);
}

publishSubscribe();



</script>
