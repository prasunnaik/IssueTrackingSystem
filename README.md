signup() {
  if (this.signupForm.invalid) {
    return;
  }

  console.log('Signup details:');
  console.log(this.signupForm.value);

  this.userService.signup(this.signupForm.value).subscribe({
    next: (response) => {
      console.log('Signup successful:', response);
      alert('Signup successful!');
    },
    error: (error) => {
      console.error('Signup failed:', error);
      alert('Signup failed!');
    }
  });
}
