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


public Issue updateIssueStatus(Long issueId, String status) {

    Issue issue = issueRepository.findById(issueId)
            .orElseThrow(() ->
                    new RuntimeException("Issue not found: " + issueId));

    issue.setStatus(status);

    return issueRepository.save(issue);
}




updateIssueStatus(issueId: number, status: string): Observable<any> {
  return this.http.put(
    `${this.apiUrl}/${issueId}/status`,
    { status: status }
  );
}
