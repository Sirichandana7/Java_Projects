# Java_Projects
#Project Title: Server Management REST API

Step 1: Set Up Your Project
1)Create a new Spring Boot project using your favorite IDE or Spring Initializer (https://start.spring.io/). Add the following dependencies:
 . Spring Web
 . Spring Data MongoDB
2)Generate the project and open it in your IDE.

Step 2: Create the Server Entity
Create a 'Server' class to represent the server objects

import org.springframework.data.annotation.Id;
import org.springframework.data.mongodb.core.mapping.Document;

@Document
public class Server {
    @Id
    private int id;
    private String name;
    private String language;
    private String framework;

  
}


Step 3: Create the Repository
Create a repository interface for the Server entity to interact with MongoDB

import org.springframework.data.mongodb.repository.MongoRepository;

public interface ServerRepository extends MongoRepository<Server, String> {
    Server findByNameContaining(String name);
}


Step 4: Create the REST Controller
Create a REST controller to define your API endpoints

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.List;
import java.util.Optional;

@RestController
@RequestMapping("/api/servers")
public class ServerController {
    @Autowired
    private ServerRepository serverRepository;

    @GetMapping
    public ResponseEntity<List<Server>> getServers(@RequestParam(required = false) String name) {
        if (name != null) {
            Server server = serverRepository.findByNameContaining(name);
            if (server != null) {
                return ResponseEntity.ok(List.of(server));
            } else {
                return ResponseEntity.notFound().build();
            }
        } else {
            List<Server> servers = serverRepository.findAll();
            return ResponseEntity.ok(servers);
        }
    }

    @PutMapping
    public ResponseEntity<Server> createServer(@RequestBody Server server) {
        Server savedServer = serverRepository.save(server);
        return ResponseEntity.status(HttpStatus.CREATED).body(savedServer);
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteServer(@PathVariable String id) {
        Optional<Server> server = serverRepository.findById(id);
        if (server.isPresent()) {
            serverRepository.deleteById(id);
            return ResponseEntity.noContent().build();
        } else {
            return ResponseEntity.notFound().build();
        }
    }
}


Step 5: Configure MongoDB
Configure your MongoDB connection properties in your application.properties file.

Step 6: Build and Run the Application
Build and run your Spring Boot application.

Step 7: Test the API
You can test the API endpoints using tools like Postman or curl here i have used postman to create and test the API end points
