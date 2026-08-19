@Override
public User login(
        String email,
        String password,
        String role) {

    User user = userRepository.findByEmail(email);

    if (user == null ||
        !user.getPassword().equals(password) ||
        !user.getRole().equals(role)) {

        throw new InvalidCredentialsException(
            "Invalid email, password or role"
        );
    }

    return user;
}

return new ResponseEntity<>(
        userService.login(
                user.getEmail(),
                user.getPassword(),
                user.getRole()
        ),
        HttpStatus.OK
);
