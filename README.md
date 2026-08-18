updateIssuePriority(
  issueId: number,
  priority: string
): Observable<any> {

  return this.http.put(
    `${this.apiUrl}/${issueId}/priority`,
    { priority: priority }
  );
} 
