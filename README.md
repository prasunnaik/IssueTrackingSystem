package com.its.issue.repository;

import java.util.List;

import org.springframework.data.jpa.repository.JpaRepository;

import com.its.issue.model.Issue;

public interface IssueRepository extends JpaRepository<Issue, Long> {

    List<Issue> findByProjectId(Long projectId);

    List<Issue> findByAssigneeId(Long assigneeId);

    List<Issue> findByStatus(String status);

    List<Issue> findByPriority(String priority);

    List<Issue> findByType(String type);
}


package com.its.issue.service;

import java.util.List;

import com.its.issue.model.Issue;

public interface IssueService {

    Issue createIssue(Issue issue);

    List<Issue> getAllIssues();

    Issue getIssueById(Long issueId);

    Issue updateIssue(Long issueId, Issue issue);

    void deleteIssue(Long issueId);

    List<Issue> getIssuesByProject(Long projectId);

    List<Issue> getIssuesByAssignee(Long assigneeId);

    List<Issue> getIssuesByStatus(String status);

    List<Issue> getIssuesByPriority(String priority);

    List<Issue> getIssuesByType(String type);
}


