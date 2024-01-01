# reliable_udp
```C
file InputFile = ./media/video.mp4
file outputFile= ./output.mp4
int Port = 12345;
```

### TO RUN SERVER:
 `gcc udp_server.c -lpthread -o udp_serv`

 `./udp_serv`

### TO RUN CLIENT:
 `gcc udp_client.c -lpthread -o udp_cli`

 `./udp_cli 127.0.0.1`

## Behavior

### General Description:
This project involves the development of sender and receiver programs that facilitate the transfer of video files using the UDP protocol in Linux/GNU C. The sender program reads video files, divides them into data chunks, and sends these chunks as UDP segments. The receiver, on the other hand, is responsible for accepting, reordering, and saving these segments to reconstruct the original file. The system employs enhanced mechanisms over standard UDP to achieve greater reliability.
### Client Module:
The sender's primary function is to transmit a video file via UDP sockets. It segments the file into 500-byte packets, incorporating retransmission for packets not acknowledged by the receiver. The transmission employs a sliding window protocol with a window size of five UDP segments, interspaced by a 0.03-second delay. The sender also listens on a specific port for acknowledgments and signals from the receiver, using a single UDP socket for both sending and receiving data. The process concludes with the sender shutting down after successful file transfer.
### Server:
The receiver binds to a specified UDP port and receives the file in 500-byte segments. Each packet is sequentially numbered to facilitate reordering and accurate reconstruction of the file at the receiver's end. The receiver operates within the same window size and delay constraints as the sender. After the complete reception of the file, the receiver saves it under a different filename and then terminates.

## Design Considerations

### Addressing UDP's Inherent Unreliability:
UDP, known for its speed, lacks in-built mechanisms to ensure data integrity and order. This is typically not an issue for applications like streaming or VoIP, where speed is prioritized over accuracy. However, for file transfers, these limitations are significant.
### Enhancing UDP Reliability:
To adapt UDP for reliable video file transfer, several strategies are employed:

1) Sequence Numbering: Packets are sequentially numbered to maintain order.
2) Selective Retransmission: Packets without acknowledgments are resent.
3) Windowed Transmission: A window of five segments is sent at a time, followed by a pause for acknowledgments and potential retransmission.
4) Reordering at Receiver: The receiver uses the sequence numbers to reorder the packets correctly.

### Client/Sender Implementation (udp_client.c):
The sender initiates a UDP socket and reads the video file, first sending the file size to the receiver. It then enters a loop where it segments the file, sends packets, waits for acknowledgments, and handles retransmissions. This loop continues until the entire file is successfully transferred.

![Server](https://github.com/mazeem77/reliable_udp/blob/main/media/client.jpeg)
### Server/Receiver Implementation (udp_server.c):
The receiver sets up a UDP socket, binds it, and then awaits data from the sender. It first receives the file size, then proceeds to receive packets, send acknowledgments, and write the data to an output file. The packets are written in the correct sequence, ensuring the integrity of the video file.

![Server](https://github.com/mazeem77/reliable_udp/blob/main/media/server.jpeg)

## Output

### Client:
![Client Output 1](https://github.com/mazeem77/reliable_udp/blob/main/media/client1.jpeg)
![Client Output 2](https://github.com/mazeem77/reliable_udp/blob/main/media/client2.jpeg)
### Server:
![Server Output 1](https://github.com/mazeem77/reliable_udp/blob/main/media/server1.jpeg)
![Server Output 2](https://github.com/mazeem77/reliable_udp/blob/main/media/server2.jpeg)