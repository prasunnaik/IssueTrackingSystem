package com.its.user.service;

import java.util.List;

import com.its.user.model.User;

public interface UserService {

    User registerUser(User user);

    User login(String email, String password);

    User getUserById(Long id);

    List<User> getAllUsers();
}

package com.its.user;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.openfeign.EnableFeignClients;

@SpringBootApplication
@EnableFeignClients
public class UserServiceApplication {

    public static void main(String[] args) {
        SpringApplication.run(UserServiceApplication.class, args);
    }
}
