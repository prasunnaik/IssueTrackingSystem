package com.its.issue.service;

import java.time.LocalDateTime;
import java.util.ArrayList;
import java.util.List;

import org.springframework.stereotype.Service;

import com.its.issue.client.ProjectClient;
import com.its.issue.exception.IssueNotFoundException;
import com.its.issue.model.Issue;
import com.its.issue.model.Project;
import com.its.issue.repository.IssueRepository;

@Service
public class IssueServiceImpl implements IssueService {

    private final IssueRepository issueRepository;
    private final ProjectClient projectClient;

    public IssueServiceImpl(
            IssueRepository issueRepository,
            ProjectClient projectClient) {

        this.issueRepository = issueRepository;
        this.projectClient = projectClient;
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

        List<Project> projects =
                projectClient.getProjectsByOwner(ownerId);

        List<Issue> issues = new ArrayList<>();

        for (Project project : projects) {

            List<Issue> projectIssues =
                    issueRepository.findByProjectId(project.getId());

            issues.addAll(projectIssues);
        }

        return issues;
    }

    @Override
    public List<Issue> getIssuesByAssignee(Long assigneeId) {

        return issueRepository.findByAssigneeId(assigneeId);
    }

    public Issue updateIssueStatus(Long issueId, String status) {

    Issue issue = issueRepository.findById(issueId)
            .orElseThrow(() ->
                    new RuntimeException("Issue not found: " + issueId));

    issue.setStatus(status);

    return issueRepository.save(issue);
}

@Override
public Issue updateIssuePriority(Long issueId, String priority) {

    Issue issue = issueRepository.findById(issueId)
            .orElseThrow(() ->
                    new RuntimeException("Issue not found: " + issueId));

    issue.setPriority(priority);

    return issueRepository.save(issue);
}

@Override
public Issue updateIssueAssignee(Long issueId, Long assigneeId) {

    Issue issue = issueRepository.findById(issueId)
            .orElseThrow(() ->
                    new RuntimeException("Issue not found: " + issueId));

    issue.setAssigneeId(assigneeId);

    return issueRepository.save(issue);
}

}
