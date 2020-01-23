# Computer Networking

Starting at the bottom and working up, 

the **physical layer** handles the transmission of raw bits over a communications link. 

The **data link layer** then collects a stream of bits into a larger aggregate called a **fram**e. Network adaptors, along with device drivers running in the node's operating system, typically implement the data link level. This means that frames, not raw bits, are actually delivered to hosts. 

The **network layer** handles routing among nodes within a packet switched network. At this layer, the unit of data exchanged among nodes is typically called a packet rather than a frame, _although they are fundamentally the same thing._ 

The lower three layers are implemented on **all network nodes**, including switches within the network and hosts connected to the exterior of the network. 

The **transport layer** then implements what we have up to this point been calling a **process-to-process** channel. Here, the unit of data exchanged is commonly called a message rather than a packet or a frame. The transport layer and higher layers typically run only on the end hosts and not on the intermediate switches or routers. There is less agreement about the definition of the top three layers, in part because they are not always all present, as we will see below.

 Skipping ahead to the top \(seventh\) layer, we find the **application layer.** Application layer protocols include things like the Hypertext Transfer Protocol \(HTTP\), which is the basis of the World Wide Web and is what enables web browsers to request pages from web servers. 

Below that, the **presentation layer** is concerned with the format of data exchanged between peers—for example, whether an integer is 16, 32, or 64 bits long, whether the most significant byte is transmitted first or last, or how a video stream is formatted. 

Finally, the **session layer** provides a name space that is used to tie together the potentially different transport streams that are part of a single application. For example, it might manage an audio stream and a video stream that are being combined in a teleconferencing application.

