getIssuesByAssignee(
  assigneeId: number
): Observable<Issue[]> {

  return this.http.get<Issue[]>(
    `${this.apiUrl}/assignee/${assigneeId}`
  );
}
