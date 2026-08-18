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
