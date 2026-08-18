openIssueDetails(issue: Issue): void {

  if (!issue.id) {
    alert('Issue ID is missing.');
    return;
  }

  this.router.navigate([
    '/issue',
    issue.id
  ]);
}
