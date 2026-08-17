import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { User } from '../models/user';
import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class UserService {

  private apiUrl = 'http://localhost:8080/api/users';

  constructor(private http: HttpClient) {}

  signup(user: User): Observable<User> {
    return this.http.post<User>(this.apiUrl, user);
  }

  login(user: User): Observable<User> {
    return this.http.post<User>(
      this.apiUrl + '/login',
      user
    );
  }

  getAllUsers(): Observable<User[]> {
    return this.http.get<User[]>(this.apiUrl);
  }

  getUserById(id: number): Observable<User> {
    return this.http.get<User>(
      this.apiUrl + '/' + id
    );
  }

  getUserIssues(id: number): Observable<any[]> {
    return this.http.get<any[]>(
      this.apiUrl + '/' + id + '/issues'
    );
  }
}
