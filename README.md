updatePriority(issue: any, priority: string): void {

  this.issueService
    .updateIssuePriority(issue.id, priority)
    .subscribe({

      next: (response) => {

        issue.priority = priority;

        this.applyFilter();

        console.log('Priority updated successfully');
      },

      error: (error) => {

        console.error(
          'Failed to update issue priority:',
          error
        );

        alert('Failed to update issue priority.');
      }

    });
}
