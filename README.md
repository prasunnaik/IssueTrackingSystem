ngOnInit(): void {

  const storedUser = localStorage.getItem('user');

  if (!storedUser) {

    this.router.navigate(['/login']);

    return;
  }

  const user = JSON.parse(storedUser);

  this.assigneeId = Number(user.id);
  this.assigneeName = user.name || 'Assignee';

  if (!this.assigneeId) {

    this.errorMessage =
      'Logged-in user information is missing.';

    return;
  }

  this.loadIssues();
}
