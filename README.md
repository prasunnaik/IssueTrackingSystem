package com.its.user;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.openfeign.EnableFeignClients;

@EnableFeignClients
@SpringBootApplication
public class UserServiceApplication {

    public static void main(String[] args) {
        SpringApplication.run(UserServiceApplication.class, args);
    }
}

package com.its.user.client;

import java.util.List;
import java.util.Map;

import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;

@FeignClient(name = "issue-service")
public interface IssueClient {

    @GetMapping("/api/issues/assignee/{assigneeId}")
    List<Map<String, Object>> getIssuesByAssignee(
            @PathVariable("assigneeId") Long assigneeId);
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

    @PostMapping
    public User registerUser(@Valid @RequestBody User user) {
        return userService.registerUser(user);
    }

    @PostMapping("/login")
    public User login(@RequestBody User user) {
        return userService.login(user.getEmail(), user.getPassword());
    }

    @GetMapping("/{id}")
    public User getUserById(@PathVariable Long id) {
        return userService.getUserById(id);
    }

    @GetMapping
    public List<User> getAllUsers() {
        return userService.getAllUsers();
    }

    // Inter-service communication with Issue Service
    @GetMapping("/{userId}/issues")
    public List<Map<String, Object>> getUserIssues(
            @PathVariable Long userId) {

        return issueClient.getIssuesByAssignee(userId);
    }
}
