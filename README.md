package com.its.issue.service;

import java.time.LocalDateTime;
import java.util.List;

import org.springframework.stereotype.Service;

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
        return issueRepository.findById(issueId).orElse(null);
    }

    @Override
    public Issue updateIssue(Long issueId, Issue issue) {

        Issue existingIssue = issueRepository.findById(issueId)
                .orElse(null);

        if (existingIssue == null) {
            return null;
        }

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
        issueRepository.deleteById(issueId);
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
