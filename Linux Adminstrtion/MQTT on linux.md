# MQTT Server Setup (PC as Server)

First, update your package lists, install Mosquitto broker and clients, enable, and start the service.

```bash
sudo apt update
sudo apt install mosquitto mosquitto-clients
sudo systemctl enable mosquitto
sudo systemctl start mosquitto
```

# Terminal 1 (Subscriber)

In one terminal window, subscribe to a topic to listen for messages.

```bash
mosquitto_sub -h localhost -t "test/topic"
```

# Terminal 2 (Publisher)

In a second terminal window, publish a message to the topic.

```bash
mosquitto_pub -h localhost -t "test/G" -m "Hello World"
```

You should see "Hello World" appear in Terminal 1.