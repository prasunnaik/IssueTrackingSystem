login(user: {
  email: string;
  password: string;
  role: string;
}): Observable<User> {

  return this.http.post<User>(
    this.apiUrl + '/login',
    user
  );
}
