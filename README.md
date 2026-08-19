package com.its.user.service;

import java.util.List;

import com.its.user.model.User;

public interface UserService {

    User registerUser(User user);

    User login(String email, String password, String role);

    User getUserById(Long id);

    User getUserByName(String name);

    List<User> getAllUsers();
}
package com.its.user.service;

import java.util.List;

import org.springframework.stereotype.Service;

import com.its.user.exception.InvalidCredentialsException;
import com.its.user.exception.UserNotFoundException;
import com.its.user.model.User;
import com.its.user.repository.UserRepository;

@Service
public class UserServiceImpl implements UserService {

    private final UserRepository userRepository;

    public UserServiceImpl(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    @Override
    public User registerUser(User user) {
        return userRepository.save(user);
    }

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


    @Override
    public User getUserById(Long id) {

        return userRepository.findById(id)
                .orElseThrow(() ->
                    new UserNotFoundException(
                        "User not found with id: " + id
                    )
                );
    }

    @Override
    public User getUserByName(String name) {

        User user = userRepository.findByName(name);

        if (user == null) {
            throw new UserNotFoundException(
                "User not found with name: " + name
            );
        }

        return user;
    }

    @Override
    public List<User> getAllUsers() {
        return userRepository.findAll();
    }
}

package com.its.user.model;

import com.fasterxml.jackson.annotation.JsonProperty;

import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;
import jakarta.persistence.Table;
import jakarta.validation.constraints.Email;
import jakarta.validation.constraints.NotBlank;

@Entity
@Table(name = "users")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @NotBlank(message = "Name is required")
    private String name;

    @NotBlank(message = "Email is required")
    @Email(message = "Please enter a valid email")
    private String email;

    @NotBlank(message = "Password is required")
    @JsonProperty(access = JsonProperty.Access.WRITE_ONLY)
    private String password;

    private String profileImage;

    @NotBlank(message = "Role is required")
    private String role;

    public User() {
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public String getProfileImage() {
        return profileImage;
    }

    public void setProfileImage(String profileImage) {
        this.profileImage = profileImage;
    }

    public String getRole() {
        return role;
    }

    public void setRole(String role) {
        this.role = role;
    }
}
