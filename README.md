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
    public List<Issue> getIssuesByOwner(Long ownerId) {

        /*
         * Owner-based issue retrieval requires
         * inter-service communication with Project Service.
         *
         * This will be implemented in the
         * Inter-Service Communication milestone.
         */

        throw new UnsupportedOperationException(
            "Owner based issue retrieval will be implemented using inter-service communication"
        );
    }

    @Override
    public List<Issue> getIssuesByAssignee(Long assigneeId) {

        return issueRepository.findByAssigneeId(assigneeId);
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

package com.its.issue.repository;

import java.util.List;

import org.springframework.data.jpa.repository.JpaRepository;

import com.its.issue.model.Issue;

public interface IssueRepository extends JpaRepository<Issue, Long> {

    List<Issue> findByProjectId(Long projectId);

    List<Issue> findByAssigneeId(Long assigneeId);
}
