@Override
public Issue updateIssueAssignee(Long issueId, Long assigneeId) {

    Issue issue = issueRepository.findById(issueId)
            .orElseThrow(() ->
                    new RuntimeException("Issue not found: " + issueId));

    issue.setAssigneeId(assigneeId);

    return issueRepository.save(issue);
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




updateIssueAssignee(
  issueId: number,
  assigneeId: number
): Observable<any> {

  return this.http.put<any>(
    `${this.apiUrl}/${issueId}/assignee/${assigneeId}`,
    {}
  );
}
