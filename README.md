package com.its.user.controller;

import java.util.List;
import java.util.Map;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import com.its.user.client.IssueClient;
import com.its.user.model.User;
import com.its.user.service.UserService;

import jakarta.validation.Valid;

@RestController
@RequestMapping("/api/users")
public class UserController {

    private final UserService userService;
    private final IssueClient issueClient;

    public UserController(UserService userService, IssueClient issueClient) {
        this.userService = userService;
        this.issueClient = issueClient;
    }

    // Get all users
    @GetMapping
    public ResponseEntity<List<User>> getAllUsers() {

        return new ResponseEntity<>(
                userService.getAllUsers(),
                HttpStatus.OK
        );
    }

    // Register a new user
    @PostMapping
    public ResponseEntity<User> registerUser(
            @Valid @RequestBody User user) {

        return new ResponseEntity<>(
                userService.registerUser(user),
                HttpStatus.CREATED
        );
    }

    // Login
    @PostMapping("/login")
    public ResponseEntity<User> login(
            @RequestBody User user) {

        return new ResponseEntity<>(
                userService.login(
                        user.getEmail(),
                        user.getPassword()
                ),
                HttpStatus.OK
        );
    }

    // Get user by ID
    @GetMapping("/{userId}")
    public ResponseEntity<User> getUserById(
            @PathVariable Long userId) {

        return new ResponseEntity<>(
                userService.getUserById(userId),
                HttpStatus.OK
        );
    }

    // Get issues assigned to a user by user ID
    // Inter-service communication: User Service -> Issue Service
    @GetMapping("/{userId}/issues")
    public ResponseEntity<List<Map<String, Object>>> getUserIssues(
            @PathVariable Long userId) {

        return new ResponseEntity<>(
                issueClient.getIssuesByAssignee(userId),
                HttpStatus.OK
        );
    }

    // Get issues assigned to a user by username
    // Inter-service communication: User Service -> Issue Service
    @GetMapping("/username/{username}/issues")
    public ResponseEntity<List<Map<String, Object>>> getUserIssuesByUsername(
            @PathVariable String username) {

        User user = userService.getUserByName(username);

        return new ResponseEntity<>(
                issueClient.getIssuesByAssignee(user.getId()),
                HttpStatus.OK
        );
    }
}


package com.its.user.model;

import com.fasterxml.jackson.annotation.JsonProperty;

import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;
import jakarta.persistence.Table;
import jakarta.validation.constraints.Email;
import jakarta.validation.constraints.NotBlank;

@Entity
@Table(name = "users")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @NotBlank(message = "Name is required")
    private String name;

    @NotBlank(message = "Email is required")
    @Email(message = "Please enter a valid email")
    private String email;

    @NotBlank(message = "Password is required")
    @JsonProperty(access = JsonProperty.Access.WRITE_ONLY)
    private String password;

    private String profileImage;

    @NotBlank(message = "Role is required")
    private String role;

    public User() {
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public String getProfileImage() {
        return profileImage;
    }

    public void setProfileImage(String profileImage) {
        this.profileImage = profileImage;
    }

    public String getRole() {
        return role;
    }

    public void setRole(String role) {
        this.role = role;
    }
}
