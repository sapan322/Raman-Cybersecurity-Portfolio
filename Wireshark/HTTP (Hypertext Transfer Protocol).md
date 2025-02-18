# Description:  

HTTP (Hypertext Transfer Protocol) is an application layer protocol used in the client-server model for communication. HTTP operates as a request-response protocol, where a web browser acts as the client and a web server as the server.  

---

## HTTP Unsecure Login Page  

HTTP is not secure by itself. Captured HTTP traffic can be intercepted and analyzed, potentially revealing sensitive data such as login credentials. We can capture login credentials from an [HTTP login page](http://testphp.vulnweb.com/login.php) using Wireshark.  

![2025-02-18 16_19_34-proxmox - Proxmox Virtual Environment](https://github.com/user-attachments/assets/decd3037-731f-4496-8ef4-9ddadfa39232)  

1. When the browser opens `http://testphp.vulnweb.com/login.php`, it sends an **HTTP GET** request to the server, requesting the web page. The server responds with the HTML page containing the login form.  
2. After entering credentials and clicking the **login** button, the browser sends an **HTTP POST** request to the server containing the entered credentials.  
3. This **POST** request is captured as an HTTP packet, which can be analyzed using Wireshark.  
4. In the **Application Layer** of the captured packet, we can see the credentials sent as **plaintext**.  



### Lessons Learned:  
- How HTTP transmits data unsecurely, without encryption.  
- How to capture and analyze HTTP traffic using Wireshark.  
- The risks associated with transmitting sensitive information over HTTP.  

---



