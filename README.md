package com.its.user.repository;

import org.springframework.data.jpa.repository.JpaRepository;

import com.its.user.model.User;

public interface UserRepository extends JpaRepository<User, Long> {

    User findByEmail(String email);

    User findByName(String name);
}


package com.its.user.service;

import java.util.List;

import com.its.user.model.User;

public interface UserService {

    User registerUser(User user);

    User login(String email, String password);

    User getUserById(Long id);

    User getUserByName(String name);

    List<User> getAllUsers();
}


package com.its.user.service;

import java.util.List;

import org.springframework.stereotype.Service;

import com.its.user.exception.InvalidCredentialsException;
import com.its.user.exception.UserNotFoundException;
import com.its.user.model.User;
import com.its.user.repository.UserRepository;

@Service
public class UserServiceImpl implements UserService {

    private final UserRepository userRepository;

    public UserServiceImpl(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    @Override
    public User registerUser(User user) {
        return userRepository.save(user);
    }

    @Override
    public User login(String email, String password) {

        User user = userRepository.findByEmail(email);

        if (user == null || !user.getPassword().equals(password)) {
            throw new InvalidCredentialsException("Invalid email or password");
        }

        return user;
    }

    @Override
    public User getUserById(Long id) {

        return userRepository.findById(id)
                .orElseThrow(() ->
                    new UserNotFoundException(
                        "User not found with id: " + id
                    )
                );
    }

    @Override
    public User getUserByName(String name) {

        User user = userRepository.findByName(name);

        if (user == null) {
            throw new UserNotFoundException(
                "User not found with name: " + name
            );
        }

        return user;
    }

    @Override
    public List<User> getAllUsers() {
        return userRepository.findAll();
    }
}



package com.its.user.controller;

import java.util.List;
import java.util.Map;

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
    public List<User> getAllUsers() {
        return userService.getAllUsers();
    }

    // Register a new user
    @PostMapping
    public User registerUser(@Valid @RequestBody User user) {
        return userService.registerUser(user);
    }

    // Login
    @PostMapping("/login")
    public User login(@RequestBody User user) {
        return userService.login(user.getEmail(), user.getPassword());
    }

    // Get user by ID
    @GetMapping("/{userId}")
    public User getUserById(@PathVariable Long userId) {
        return userService.getUserById(userId);
    }

    // Get issues assigned to a user by user ID
    // Inter-service communication: User Service -> Issue Service
    @GetMapping("/{userId}/issues")
    public List<Map<String, Object>> getUserIssues(
            @PathVariable Long userId) {

        return issueClient.getIssuesByAssignee(userId);
    }

    // Get issues assigned to a user by username
    // Here username is the existing User.name field
    // Inter-service communication: User Service -> Issue Service
    @GetMapping("/username/{username}/issues")
    public List<Map<String, Object>> getUserIssuesByUsername(
            @PathVariable String username) {

        User user = userService.getUserByName(username);

        return issueClient.getIssuesByAssignee(user.getId());
    }
}
