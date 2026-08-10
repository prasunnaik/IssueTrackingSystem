package com.its.issue.exception;

public class IssueNotFoundException extends RuntimeException {

    public IssueNotFoundException(String message) {
        super(message);
    }
}

package com.its.issue.exception;

import java.util.HashMap;
import java.util.Map;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.MethodArgumentNotValidException;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RestControllerAdvice;

@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(IssueNotFoundException.class)
    public ResponseEntity<String> handleIssueNotFound(
            IssueNotFoundException exception) {

        return new ResponseEntity<>(
                exception.getMessage(),
                HttpStatus.NOT_FOUND
        );
    }

    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<Map<String, String>> handleValidationErrors(
            MethodArgumentNotValidException exception) {

        Map<String, String> errors = new HashMap<>();

        exception.getBindingResult()
                .getFieldErrors()
                .forEach(error ->
                        errors.put(
                                error.getField(),
                                error.getDefaultMessage()
                        )
                );

        return new ResponseEntity<>(
                errors,
                HttpStatus.BAD_REQUEST
        );
    }
}


package com.its.issue.service;

import java.time.LocalDateTime;
import java.util.List;

import org.springframework.stereotype.Service;

import com.its.issue.exception.IssueNotFoundException;
import com.its.issue.model.Issue;
import com.its.issue.repository.IssueRepository;

@Service
public class IssueServiceImpl implements IssueService {

    private final IssueRepository issueRepository;

    public IssueServiceImpl(IssueRepository issueRepository) {
        this.issueRepository = issueRepository;
    }

    @Override
    public Issue createIssue(Issue issue) {

        issue.setCreatedDate(LocalDateTime.now());
        issue.setLastUpdatedDate(LocalDateTime.now());

        return issueRepository.save(issue);
    }

    @Override
    public List<Issue> getAllIssues() {
        return issueRepository.findAll();
    }

    @Override
    public Issue getIssueById(Long issueId) {

        return issueRepository.findById(issueId)
                .orElseThrow(() ->
                        new IssueNotFoundException(
                                "Issue not found with id: " + issueId
                        )
                );
    }

    @Override
    public Issue updateIssue(Long issueId, Issue issue) {

        Issue existingIssue = issueRepository.findById(issueId)
                .orElseThrow(() ->
                        new IssueNotFoundException(
                                "Issue not found with id: " + issueId
                        )
                );

        existingIssue.setSummary(issue.getSummary());
        existingIssue.setDescription(issue.getDescription());
        existingIssue.setPriority(issue.getPriority());
        existingIssue.setAssigneeId(issue.getAssigneeId());
        existingIssue.setStatus(issue.getStatus());
        existingIssue.setProjectId(issue.getProjectId());
        existingIssue.setSprint(issue.getSprint());
        existingIssue.setStoryPoint(issue.getStoryPoint());
        existingIssue.setTags(issue.getTags());
        existingIssue.setType(issue.getType());
        existingIssue.setLastUpdatedDate(LocalDateTime.now());

        return issueRepository.save(existingIssue);
    }

    @Override
    public void deleteIssue(Long issueId) {

        Issue existingIssue = issueRepository.findById(issueId)
                .orElseThrow(() ->
                        new IssueNotFoundException(
                                "Issue not found with id: " + issueId
                        )
                );

        issueRepository.delete(existingIssue);
    }

    @Override
    public List<Issue> getIssuesByProject(Long projectId) {
        return issueRepository.findByProjectId(projectId);
    }

    @Override
    public List<Issue> getIssuesByAssignee(Long assigneeId) {
        return issueRepository.findByAssigneeId(assigneeId);
    }

    @Override
    public List<Issue> getIssuesByStatus(String status) {
        return issueRepository.findByStatus(status);
    }

    @Override
    public List<Issue> getIssuesByPriority(String priority) {
        return issueRepository.findByPriority(priority);
    }

    @Override
    public List<Issue> getIssuesByType(String type) {
        return issueRepository.findByType(type);
    }
}


