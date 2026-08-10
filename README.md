package com.its.user.service;

import java.util.List;

import com.its.user.model.User;

public interface UserService {

    User registerUser(User user);

    User login(String email, String password);

    User getUserById(Long id);

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
    public List<User> getAllUsers() {
        return userRepository.findAll();
    }
}


package com.its.user.controller;

import java.util.List;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import com.its.user.model.User;
import com.its.user.service.UserService;

import jakarta.validation.Valid;

@RestController
@RequestMapping("/api/users")
public class UserController {

    private final UserService userService;

    public UserController(UserService userService) {
        this.userService = userService;
    }

    @GetMapping
    public ResponseEntity<List<User>> getAllUsers() {

        List<User> users = userService.getAllUsers();

        return new ResponseEntity<>(
                users,
                HttpStatus.OK
        );
    }

    @PostMapping
    public ResponseEntity<User> registerUser(
            @Valid @RequestBody User user) {

        User savedUser = userService.registerUser(user);

        return new ResponseEntity<>(
                savedUser,
                HttpStatus.CREATED
        );
    }

    @PostMapping("/login")
    public ResponseEntity<User> login(
            @RequestBody User user) {

        User loggedInUser =
                userService.login(
                        user.getEmail(),
                        user.getPassword()
                );

        return new ResponseEntity<>(
                loggedInUser,
                HttpStatus.OK
        );
    }

    @GetMapping("/{userId}")
    public ResponseEntity<User> getUserById(
            @PathVariable Long userId) {

        User user = userService.getUserById(userId);

        return new ResponseEntity<>(
                user,
                HttpStatus.OK
        );
    }
}
