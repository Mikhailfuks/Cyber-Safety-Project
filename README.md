using System;
using System.Net;
using System.Net.Sockets;
using System.Text;

public class CyberSafetyProject
{
    private const int PortNumber = 8080; // Choose a port number (must be available)

    public static void Main(string[] args)
    {
        Console.WriteLine("Welcome to the Cyber Safety Project!");
        Console.WriteLine("This program demonstrates some basic network security concepts.");

        // Create a TCP listener to listen for incoming connections
        TcpListener listener = new TcpListener(IPAddress.Any, PortNumber);
        listener.Start();
        Console.WriteLine($"Listening on port {PortNumber}...");

        // Accept a connection from a client
        TcpClient client = listener.AcceptTcpClient();
        Console.WriteLine("Client connected.");

        // Create network streams for reading and writing
        NetworkStream stream = client.GetStream();
        StreamReader reader = new StreamReader(stream, Encoding.ASCII);
        StreamWriter writer = new StreamWriter(stream, Encoding.ASCII);

        // Handle client communication
        try
        {
            while (true)
            {
                string data = reader.ReadLine();

                if (data != null)
                {
                    Console.WriteLine($"Client sent: {data}");

                    // Perform security checks (example: detect potential threats)
                    if (data.ToLower().Contains("password") || data.ToLower().Contains("credit card"))
                    {
                        writer.WriteLine("Warning: Sensitive information detected. Be cautious!");
                    }
                    else
                    {
                        // Example: Echo back the data (replace with more complex functionality)
                        writer.WriteLine($"You sent: {data}");
                    }

                    writer.Flush(); // Send response to the client
                }
                else
                {
                    Console.WriteLine("Client disconnected.");
                    break;
                }
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error: {ex.Message}");
        }

        // Close connections
        client.Close();
        listener.Stop();
    }
}
