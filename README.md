import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class IssueService {

  private apiUrl = 'http://localhost:8083/api/issues';

  constructor(private http: HttpClient) {}

  getIssuesByProject(projectId: number): Observable<any[]> {
    return this.http.get<any[]>(
      `${this.apiUrl}/project/${projectId}`
    );
  }
}
