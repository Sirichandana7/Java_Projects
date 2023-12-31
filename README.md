# Java_Project using API
Task 1 : Java REST API

Step 1: Set Up Your Project

1)Create a new Spring Boot project using Spring Initializer (https://start.spring.io/). Add the following dependencies:
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
	
        @GetMapping("/findByName/{name}")
	
	    public ResponseEntity<List<Server>> findServersByName(@PathVariable("name") String name) {
	        List<Server> server =  serverRepository.findByNameContaining(name);
	        return server.size() != 0?
	                new ResponseEntity<>(server, HttpStatus.OK) :
	                new ResponseEntity<>(HttpStatus.NOT_FOUND);
	    }
    }
    }



Step 5: Configure MongoDB

Configure your MongoDB connection properties in your application.properties file.

Step 6: Build and Run the Application

Build and run your Spring Boot application.

Step 7: Test the API

You can test the API endpoints using tools like Postman or curl here i have used postman to create and test the API end points

SCREENSHOTS OF INPUT AND OUTPUT :

![image](https://github.com/Sirichandana7/Java_Projects/assets/83815075/60499731-8804-437b-ad8d-b9900a74acbb)

GET servers : Should return all the servers if no parameters are passed. When server id
is passed as a parameter - return a single server or 404 if there’s no such a server.

![image](https://github.com/Sirichandana7/Java_Projects/assets/83815075/f0a7191e-ccd5-4ffa-a2af-a36585eaf83c)

![image](https://github.com/Sirichandana7/Java_Projects/assets/83815075/4077b9aa-f979-49b3-9aa9-a4e550ed5290)


PUT a server :  The server object is passed as a json-encoded message body.

![image](https://github.com/Sirichandana7/Java_Projects/assets/83815075/63a33c45-fc83-46ec-b9ae-8486983ef08f)


DELETE a server :  The parameter is a server ID.
Here i Have deleted  by id = 2

![image](https://github.com/Sirichandana7/Java_Projects/assets/83815075/110e3356-e743-4f72-ab20-2d60753bcb85)


![image](https://github.com/Sirichandana7/Java_Projects/assets/83815075/c43d4076-bb32-4d1a-8bb4-6645d799a4ff)

GET (find) servers by name : The parameter is a string. Must check if a server name
contains this string and return one or more servers found. Return 404 if nothing is found.

![image](https://github.com/Sirichandana7/Java_Projects/assets/83815075/cc74c5e9-831b-46db-a18a-df3708410ed9)


![image](https://github.com/Sirichandana7/Java_Projects/assets/83815075/c9da43e1-8fb8-468e-9be2-f28e9a3f0c75)






