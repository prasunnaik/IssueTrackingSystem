ngOnInit(): void {

  const storedUser = localStorage.getItem('user');

  if (!storedUser) {

    this.router.navigate(['/login']);

    return;
  }

  const user = JSON.parse(storedUser);

  this.ownerId = Number(user.id);
  this.ownerName = user.name || 'Project Owner';

  if (!this.ownerId) {

    this.errorMessage =
      'Logged-in user information is missing.';

    return;
  }

  this.loadProjects();
}
