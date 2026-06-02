# Rabbit - C# RabbitMQ Messaging Project

A C# demonstration project for sending and receiving messages via **RabbitMQ**. This repository showcases how to work with RabbitMQ using C# for message queue operations, including publishing messages to and consuming messages from RabbitMQ brokers.

## 📋 Overview

This project contains two main components:

- **Send**: A console application that publishes messages to a RabbitMQ queue
- **Receive**: A console application that consumes messages from a RabbitMQ queue

## 🏗️ Project Structure

```
Rabbit/
├── Send/                    # Message publisher project
│   ├── Send.cs             # Main publisher implementation
│   ├── Send.csproj         # Project file
│   ├── bin/                # Compiled binaries
│   └── obj/                # Build artifacts
├── Receive/                 # Message consumer project
│   ├── Receive.cs          # Main consumer implementation
│   ├── Receive.csproj      # Project file
│   └── obj/                # Build artifacts
├── Send.sln                # Visual Studio solution file
└── README.md               # This file
```

## 🛠️ Technology Stack

- **Language**: C# (.NET Core 1.1)
- **Message Broker**: RabbitMQ
- **NuGet Dependencies**: RabbitMQ.Client (v4.1.3)

## 📦 Prerequisites

Before running this project, ensure you have:

- **.NET Core 1.1 SDK** or later installed
- **RabbitMQ Server** running and accessible
- Default RabbitMQ connection credentials configured

## ⚙️ Configuration

The project uses the following RabbitMQ connection settings:

```csharp
HostName = "localhost"
Port = 5627
UserName = "admin"
Password = "password1234$"
```

**⚠️ Security Note**: The credentials are hardcoded in this demo project. For production use, consider storing sensitive information in environment variables or a secure configuration file.

## 🚀 Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/gertcoet/Rabbit.git
cd Rabbit
```

### 2. Restore Dependencies

```bash
dotnet restore
```

### 3. Build the Solution

```bash
dotnet build
```

## 📤 Sending Messages (Send Project)

The **Send** project demonstrates how to publish a message to a RabbitMQ queue.

### Key Features:

- Creates a connection to RabbitMQ
- Declares a queue named "hello"
- Publishes a test message: "Hello World!"
- Waits for user input before closing

### Running the Send Application:

```bash
cd Send
dotnet run
```

The application will:
1. Connect to the RabbitMQ broker
2. Declare a queue (if it doesn't exist)
3. Send the message "Hello World!" to the queue
4. Display: `[x] Sent Hello World!`
5. Wait for you to press Enter before exiting

### Code Example:

```csharp
var factory = new ConnectionFactory() 
{
    HostName = "localhost",
    Port = 5627, 
    UserName = "admin",
    Password = "password1234$"
};

using(var connection = factory.CreateConnection())
using(var channel = connection.CreateModel())
{
    channel.QueueDeclare(queue: "hello", durable: false, exclusive: false, 
                         autoDelete: false, arguments: null);
    
    string message = "Hello World!";
    var body = Encoding.UTF8.GetBytes(message);
    
    channel.BasicPublish(exchange: "", routingKey: "hello", basicProperties: null, body: body);
    Console.WriteLine(" [x] Sent {0}", message);
}
```

## 📥 Receiving Messages (Receive Project)

The **Receive** project demonstrates how to consume messages from a RabbitMQ queue.

**Note**: The Receive.cs file appears to be incomplete in the current repository. To implement a consumer, you would typically:

1. Create a connection to RabbitMQ
2. Declare the same queue
3. Set up a consumer
4. Process incoming messages

### Example Implementation Structure:

```csharp
using System;
using RabbitMQ.Client;
using RabbitMQ.Client.Events;
using System.Text;

class Receive
{
    public static void Main()
    {
        var factory = new ConnectionFactory() 
        {
            HostName = "localhost",
            Port = 5627,
            UserName = "admin",
            Password = "password1234$"
        };

        using(var connection = factory.CreateConnection())
        using(var channel = connection.CreateModel())
        {
            channel.QueueDeclare(queue: "hello", durable: false, exclusive: false, 
                                 autoDelete: false, arguments: null);

            var consumer = new EventingBasicConsumer(channel);
            consumer.Received += (model, ea) =>
            {
                var body = ea.Body;
                var message = Encoding.UTF8.GetString(body);
                Console.WriteLine(" [x] Received {0}", message);
            };

            channel.BasicConsume(queue: "hello", noAck: true, consumerTag: "", 
                                consumer: consumer);

            Console.WriteLine(" [*] Waiting for messages. Press [enter] to exit.");
            Console.ReadLine();
        }
    }
}
```

## 🔄 Usage Workflow

1. **Start RabbitMQ Server** on your system
2. **Run the Send application** in one terminal to publish a message
3. **Run the Receive application** in another terminal to consume the message
4. Verify that the message is successfully transmitted and received

## 📚 References

- [RabbitMQ Official Documentation](https://www.rabbitmq.com/documentation.html)
- [RabbitMQ .NET Client Documentation](https://www.rabbitmq.com/dotnet-api-guide.html)
- [RabbitMQ Tutorials](https://www.rabbitmq.com/getstarted.html)

## 📝 License

This project is provided as a demonstration and test project for RabbitMQ messaging with C#.

## 👤 Author

Created by [gertcoet](https://github.com/gertcoet)

---

**Last Updated**: 2020-08-24

For questions, issues, or contributions, please feel free to open an issue or submit a pull request.
