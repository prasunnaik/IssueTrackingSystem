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

        List<Issue> issues = issueService.getAllIssues();

        return new ResponseEntity<>(
                issues,
                HttpStatus.OK
        );
    }

    @GetMapping("/{issueId}")
    public ResponseEntity<Issue> getIssueById(
            @PathVariable Long issueId) {

        Issue issue = issueService.getIssueById(issueId);

        return new ResponseEntity<>(
                issue,
                HttpStatus.OK
        );
    }

    @PutMapping("/{issueId}")
    public ResponseEntity<Issue> updateIssue(
            @PathVariable Long issueId,
            @Valid @RequestBody Issue issue) {

        Issue updatedIssue =
                issueService.updateIssue(issueId, issue);

        return new ResponseEntity<>(
                updatedIssue,
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

        List<Issue> issues =
                issueService.getIssuesByProject(projectId);

        return new ResponseEntity<>(
                issues,
                HttpStatus.OK
        );
    }

    @GetMapping("/assignee/{assigneeId}")
    public ResponseEntity<List<Issue>> getIssuesByAssignee(
            @PathVariable Long assigneeId) {

        List<Issue> issues =
                issueService.getIssuesByAssignee(assigneeId);

        return new ResponseEntity<>(
                issues,
                HttpStatus.OK
        );
    }

    @GetMapping("/status/{status}")
    public ResponseEntity<List<Issue>> getIssuesByStatus(
            @PathVariable String status) {

        List<Issue> issues =
                issueService.getIssuesByStatus(status);

        return new ResponseEntity<>(
                issues,
                HttpStatus.OK
        );
    }

    @GetMapping("/priority/{priority}")
    public ResponseEntity<List<Issue>> getIssuesByPriority(
            @PathVariable String priority) {

        List<Issue> issues =
                issueService.getIssuesByPriority(priority);

        return new ResponseEntity<>(
                issues,
                HttpStatus.OK
        );
    }

    @GetMapping("/type/{type}")
    public ResponseEntity<List<Issue>> getIssuesByType(
            @PathVariable String type) {

        List<Issue> issues =
                issueService.getIssuesByType(type);

        return new ResponseEntity<>(
                issues,
                HttpStatus.OK
        );
    }
}
