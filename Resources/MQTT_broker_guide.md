---

# **MQTT Broker Setup Guide**

## **Introduction**

In this guide, we will walk you through the process of setting up a local MQTT broker using **Mosquitto**. MQTT is a lightweight messaging protocol that is perfect for sending commands between the glove (ESP32) and the robot (ESP32) over a network. The broker acts as the intermediary between the two ESP32 devices, allowing them to communicate wirelessly.

### **Why Use MQTT?**

MQTT is widely used for IoT applications due to its lightweight and efficient communication model. It uses the **publish/subscribe model**, which makes it ideal for projects like this, where the glove (controller) needs to send commands to the robot over a network.

---

## **Setting Up a Local MQTT Broker Using Mosquitto**

### **Step 1: Install Mosquitto on Your Local Machine**

Mosquitto is an open-source MQTT broker that can run on most operating systems. Below are the installation steps for both **Windows** and **Linux**.

### **For Windows**

1. **Download Mosquitto Installer:**
   - Go to the official Mosquitto website: [Mosquitto Downloads](https://mosquitto.org/download/)
   - Download the installer for Windows (choose the `.exe` version).

2. **Run the Installer:**
   - Follow the installation instructions. You can leave the default settings unless you need to customize them.
   - After installation, Mosquitto will be installed in your `Program Files` directory.

3. **Start the Mosquitto Broker:**
   - Open a **Command Prompt** as Administrator.
   - Navigate to the folder where Mosquitto is installed, typically `C:\Program Files\mosquitto\`.
   - Run the following command to start the broker:
     ```bash
     mosquitto
     ```
   - The broker will now be running on **localhost** (127.0.0.1) and port **1883**, which is the default MQTT port.

4. **Verify the Installation:**
   - To verify that the broker is running, open a browser and go to `http://localhost:1883`. You should see a Mosquitto WebSocket interface or a blank page (which means the broker is working).
   - Alternatively, you can use an MQTT client like **MQTT.fx** or **MQTT Explorer** to connect to the broker using `localhost` and port `1883`.

### **For Linux (Ubuntu/Debian)**

1. **Install Mosquitto:**
   Open a terminal and run the following command to install Mosquitto:
   ```bash
   sudo apt update
   sudo apt install mosquitto mosquitto-clients
   ```

2. **Start the Mosquitto Service:**
   - After installation, start the Mosquitto service with the following command:
     ```bash
     sudo systemctl start mosquitto
     ```

3. **Enable Mosquitto to Start on Boot:**
   - To make sure Mosquitto starts automatically after a reboot, run:
     ```bash
     sudo systemctl enable mosquitto
     ```

4. **Verify the Installation:**
   - Check that the Mosquitto service is running:
     ```bash
     sudo systemctl status mosquitto
     ```
   - The broker will now be running on **localhost** (127.0.0.1) and port **1883** by default.

---

## **Step 2: Configure Mosquitto (Optional)**

By default, Mosquitto allows connections without authentication and is configured to work on **localhost**. However, you can edit the configuration file if you want to:

1. **Locate the Configuration File:**
   - On Windows, the Mosquitto configuration file is typically found at `C:\Program Files\mosquitto\mosquitto.conf`.
   - On Linux, the configuration file is located at `/etc/mosquitto/mosquitto.conf`.

2. **Edit the Configuration File:**
   You can edit this file to configure various settings, including enabling authentication, setting a different port, and changing logging levels.

   Example:
   ```bash
   # Set the MQTT port
   listener 1883

   # Enable persistence
   persistence true
   persistence_location /var/lib/mosquitto/

   # Set the username and password (optional)
   allow_anonymous false
   password_file /etc/mosquitto/passwd
   ```

3. **Restart Mosquitto:**
   After making changes to the configuration file, restart the Mosquitto service:
   - On Windows, close the Command Prompt and restart Mosquitto via the program.
   - On Linux, run:
     ```bash
     sudo systemctl restart mosquitto
     ```

---

## **Step 3: Test MQTT Broker Connectivity**

Once the broker is running, it’s time to test the connectivity. You can use MQTT client tools like **MQTT.fx** or **MQTT Explorer** to publish and subscribe to topics.

1. **Install MQTT Client:**
   - Download and install **MQTT.fx** (available for both Windows and macOS) or **MQTT Explorer** from their respective websites.

2. **Connect to the MQTT Broker:**
   - Open your MQTT client and create a new connection.
   - Enter the following connection settings:
     - **Host:** `localhost` or `127.0.0.1`
     - **Port:** `1883` (default MQTT port)
     - **Client ID:** (can be any name)

3. **Test Publishing and Subscribing:**
   - **Publish a message** to the topic `robot/commands` to simulate sending commands to the robot:
     - Topic: `robot/commands`
     - Message: `left` (for example)
   - **Subscribe** to the topic `robot/commands` to listen for incoming messages.
   - If everything is set up correctly, you should see the message "left" in your subscriber client, confirming that your broker is functioning.

---

## **Step 4: Update the ESP32 Code with MQTT Settings**

Now that your local MQTT broker is running, you need to update the **ESP32 code** on both the glove and the robot to connect to the broker.

1. **Modify the MQTT Broker IP in the Code:**
   In both the **glove ESP32** and **robot ESP32** code (`gesture_detection.ino` and `robot_control.ino`), replace the MQTT server IP address with your local IP address (the machine running Mosquitto). If you’re running Mosquitto on the same machine as the ESP32, you can use `127.0.0.1` or `localhost`.

   Example:
   ```cpp
   const char* mqtt_server = "127.0.0.1";  // Replace with your local MQTT broker IP address
   ```

2. **Upload the Code:**
   Upload the updated code to the ESP32 devices using the Arduino IDE, and then test the communication between the glove and the robot.

---

## **Conclusion**

You have successfully set up a local MQTT broker using **Mosquitto** and connected your ESP32 devices to it. This MQTT broker will facilitate wireless communication between the glove and the robot, enabling gesture-based control.

---

For more advanced features, you can explore cloud-based MQTT brokers such as **HiveMQ** or **CloudMQTT**, but this guide focuses on the local setup using Mosquitto for simplicity and efficiency.

