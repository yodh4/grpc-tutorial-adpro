Reflection
1. What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable?

    Perbedaan utama antara unary, server streaming, dan bi-directional streaming terletak pada bagaimana data diproses selama procedure call berlangsung. 

    Unary RPC merupakan metode paling simple dimana client akan mengirim single request kemudian server akan memproses request tersebut dan mengirim response ke client. Method ini cocok dipakai untuk request yang simpel seperti melakukan query ke database , fetching data user, atau pengisian form.
    
    Server streaming RPC merupakan metode dimana client akan mengirim single request dan server akan mengirim response secara terus menerus secara asinkronus. Method ini cocok digunakan jika server perlu mengirim data yang banyak berkali-kali seperti saat menampilkan feeds sosmed
    
    Bi-directional streaming RPC merupakan metode dimana klien dan server saling mengirim data secara asinkronus. Method ini cocok digunakan pada aplikasi yang dapat digunakan secara collaborative seperti google docs atau online multiplayer game
<br></br>
2. What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?

    kita bisa mengimplementasikan service gRPC di rust dengan tetap mempertimbangkan sisi keamanan seperti authentication, authorization, dan data encryption. Untuk autentikasi kita bisa mengimplementasikan sistem token dengan menggunakan library `jsonwebtoken`, untuk otorisasi kita bisa mengimplementasikan sistem RBAC(Role-Based Access Control) dengan menggunakan library `authorization` atau `rbac`. Untuk enkripsi data kita bisa menggunakan library-library yang umum digunakan seperti `ring` atau `openssl`
<br></br>
3. What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?

    Salah satu isu yang mungkin muncul dari bidirectional streaming adalah kemungkinan terjadinya performance issue dari aplikasi yang kita buat. Jika menggunakan skenario chat application maka issue yang mungkin muncul adalah saat client mengirim data secara terus menerus sampai mengalahkan kecepatan server me-response data tersebut maka akan terjadi buffering dari pihak client karena menunggu respons dari server, bahkan kemungkinan terburuknya aplikasi bisa saja mengalami crash 
<br></br>
4. What are the advantages and disadvantages of using the tokio_stream::wrappers::ReceiverStream for streaming responses in Rust gRPC services?

    Keuntungan:
    - Backpressure handling, `ReceiverStream` dapat mengoptimalkan backpressure handling dengan mengirim sinyal ke client apabila rate data yang dikirim terlalu cepat untuk mencegah sistem mengalami buffering atau crash
    - Asynchronous yang berguna untuk mempercepat pengolahan data yang dikirim oleh client

    Kekurangan:
    - Kompleksitas program kita akan bertambah dan kita harus memahami dulu asynchronous programming dan konsep `stream` agar dapat menggunakan `tokio_stream::wrappers::ReceiverStream`
<br></br>
5. In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?
    
    Protokol gPRC menggunakan protocol buffers sebagai Interface Definition Language (IDL) untuk mendefinisikan service yang digunakan, dengan menggunakan protocol buffers maintainability dan extensibility dapat dilakukan dengan mudah karena kode ini terpisah dari business logic aplikasi kita sehingga perubahan atau penambahan fitur tidak akan mengganggu business logic program kita.
<br></br>
6. In the MyPaymentService implementation, what additional steps might be necessary to handle more complex payment processing logic?

    Salah satu cara yang penting untuk dilakukan saat payment processing sudah menjadi lebih kompleks adalah dengan menambahkan validator saat menerima request, kita dapat membuat method validator yang akan dipanggil di dalam fungsi `process_payment` untuk memvalidasi request seperti mengecek nominal payment atau mengecek nilai kurensi yang digunakan.
<br></br>
7. What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?

    Pengadopsian gRPC sebagai communication protocol sangat mendukung interoperability karena protocol buffer yang digunakan pada gRPC merupakan language netral sehingga memungkinkan client dan server ditulis dengan programming language yang berbeda, selain itu gRPC menggunakan binary serialization yang memungkinkan transfer data menjadi lebih cepat. . 
<br></br>
8. What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?

    Kelebihan dari HTTP/2 dibandingkan dengan dengan HTTP/1.1 adalah kemampuan untuk hanya menggunakan 1 koneksi pada proses transfer data yang terjadi berkali-kali, hal ini membuat proses transfer data menjadi lebih cepat karena tidak perlu membuat jalur koneksi baru pada setiap terjadi pentransferan data antara client dan server. Kekurangan HTTP/2 adalah masih kurang umum jika dibandingkan dengan HTTP/1 sehingga belum semua teknologi/library yang ada mensupport HTTP/2  
<br></br>
9. How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness?

    Dalam hal real time communication dan responsiveness, gRPC akan unggul dibandingkan dengan REST API karena bi-directional streaming pada gRPC hanya akan membuat satu koneksi dan memanfaatkan koneksi tersebut untuk melakukan transfer data antara client dan server sedangkan pada REST API setiap terjadi transfer data antara client dan server harus dibuat jalur koneksi baru yang membuat prosesnya menjadi lebih lama
<br></br>
10. What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?

    Implikasi dari pendekatan schema based pada gRPC adalah data yang dikirim sudah didefinisikan pada file .proto, hal ini menyebabkan proses pengiriman data lebih terstruktur jika dibandingkan dengan pengiriman data menggunakan JSON yang tidak terdefinisi di awal

