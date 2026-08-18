updateIssueStatus(issueId: number, status: string): Observable<any> {
  return this.http.put(
    `${this.apiUrl}/${issueId}`,
    { status: status }
  );
}
