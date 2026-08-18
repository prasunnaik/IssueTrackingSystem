import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class IssueService {

  private apiUrl = 'http://localhost:8083/api/issues';

  constructor(
    private http: HttpClient
  ) {}

  getIssuesByProject(
    projectId: number
  ): Observable<any[]> {

    return this.http.get<any[]>(
      `${this.apiUrl}/project/${projectId}`
    );

  }


  getIssueById(
    issueId: number
  ): Observable<any> {

    return this.http.get<any>(
      `${this.apiUrl}/${issueId}`
    );

  }


  updateIssue(
    issueId: number,
    issue: any
  ): Observable<any> {

    return this.http.put<any>(
      `${this.apiUrl}/${issueId}`,
      issue
    );

  }


  deleteIssue(
    issueId: number
  ): Observable<any> {

    return this.http.delete<any>(
      `${this.apiUrl}/${issueId}`
    );

  }

  updateIssueStatus(issueId: number, status: string): Observable<any> {
  return this.http.put(
    `${this.apiUrl}/${issueId}/status`,
    { status: status }
  );
}

  updateIssuePriority(
  issueId: number,
  priority: string
): Observable<any> {

  return this.http.put(
    `${this.apiUrl}/${issueId}/priority`,
    { priority: priority }
  );
} 

  updateIssueAssignee(
  issueId: number,
  assigneeId: number
): Observable<any> {

  return this.http.put<any>(
    `${this.apiUrl}/${issueId}/assignee/${assigneeId}`,
    {}
  );
}

}

package com.its.issue.controller;

import java.util.List;
import java.util.Map;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.CrossOrigin;
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

@CrossOrigin(origins = "hjttp://localhost:4300")
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

    @PutMapping("/{issueId}/status")
public ResponseEntity<Issue> updateIssueStatus(
        @PathVariable Long issueId,
        @RequestBody Map<String, String> request) {

    String status = request.get("status");

    Issue updatedIssue =
            issueService.updateIssueStatus(issueId, status);

    return new ResponseEntity<>(
            updatedIssue,
            HttpStatus.OK
    );
}

@PutMapping("/{issueId}/priority")
public ResponseEntity<Issue> updateIssuePriority(
        @PathVariable Long issueId,
        @RequestBody Map<String, String> request) {

    String priority = request.get("priority");

    Issue updatedIssue =
            issueService.updateIssuePriority(issueId, priority);

    return new ResponseEntity<>(
            updatedIssue,
            HttpStatus.OK
    );
}

@PutMapping("/{issueId}/assignee/{assigneeId}")
public ResponseEntity<Issue> updateIssueAssignee(
        @PathVariable Long issueId,
        @PathVariable Long assigneeId) {

    return new ResponseEntity<>(
            issueService.updateIssueAssignee(issueId, assigneeId),
            HttpStatus.OK
    );
}


}
