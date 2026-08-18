updateAssignee(issue: any, assigneeId: number): void {

  this.issueService.updateIssueAssignee(issue.id, assigneeId)
    .subscribe({
      next: (response) => {
        issue.assigneeId = response.assigneeId;
      },
      error: (error) => {
        console.error('Failed to update issue assignee:', error);
        alert('Failed to update issue assignee.');
      }
    });
}


<label>Assignee:</label>

<select
  [ngModel]="issue.assigneeId"
  (ngModelChange)="updateAssignee(issue, $event)"
>
  <option [ngValue]="1">1</option>
  <option [ngValue]="2">2</option>
  <option [ngValue]="3">3</option>
  <option [ngValue]="4">4</option>
</select>



