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

package com.its.issue.controller;

import java.util.List;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.PutMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import com.its.issue.model.Issue;
import com.its.issue.service.IssueService;

import jakarta.validation.Valid;

@RestController
@RequestMapping("/api/issues")
public class IssueController {

    private final IssueService issueService;

    public IssueController(IssueService issueService) {
        this.issueService = issueService;
    }

    @PostMapping
    public ResponseEntity<Issue> createIssue(
            @Valid @RequestBody Issue issue) {

        Issue savedIssue = issueService.createIssue(issue);

        return new ResponseEntity<>(
                savedIssue,
                HttpStatus.CREATED
        );
    }

    @GetMapping
    public ResponseEntity<List<Issue>> getAllIssues() {

        return new ResponseEntity<>(
                issueService.getAllIssues(),
                HttpStatus.OK
        );
    }

    @GetMapping("/{issueId}")
    public ResponseEntity<Issue> getIssueById(
            @PathVariable Long issueId) {

        return new ResponseEntity<>(
                issueService.getIssueById(issueId),
                HttpStatus.OK
        );
    }

    @PutMapping("/{issueId}")
    public ResponseEntity<Issue> updateIssue(
            @PathVariable Long issueId,
            @Valid @RequestBody Issue issue) {

        return new ResponseEntity<>(
                issueService.updateIssue(issueId, issue),
                HttpStatus.OK
        );
    }

    @DeleteMapping("/{issueId}")
    public ResponseEntity<Void> deleteIssue(
            @PathVariable Long issueId) {

        issueService.deleteIssue(issueId);

        return new ResponseEntity<>(
                HttpStatus.NO_CONTENT
        );
    }

    @GetMapping("/project/{projectId}")
    public ResponseEntity<List<Issue>> getIssuesByProject(
            @PathVariable Long projectId) {

        return new ResponseEntity<>(
                issueService.getIssuesByProject(projectId),
                HttpStatus.OK
        );
    }

    @GetMapping("/owner/{ownerId}")
    public ResponseEntity<List<Issue>> getIssuesByOwner(
            @PathVariable Long ownerId) {

        return new ResponseEntity<>(
                issueService.getIssuesByOwner(ownerId),
                HttpStatus.OK
        );
    }

    @GetMapping("/assignee/{assigneeId}")
    public ResponseEntity<List<Issue>> getIssuesByAssignee(
            @PathVariable Long assigneeId) {

        return new ResponseEntity<>(
                issueService.getIssuesByAssignee(assigneeId),
                HttpStatus.OK
        );
    }
}
