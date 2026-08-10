package com.its.user.service;

import com.its.user.model.User;

public interface UserService {

    User registerUser(User user);

    User login(String email, String password);

    User getUserById(Long id);
}


package com.its.user.service;

import org.springframework.stereotype.Service;

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
    public User login(String email, String password) {

        User user = userRepository.findByEmail(email);

        if (user != null && user.getPassword().equals(password)) {
            return user;
        }

        return null;
    }

    @Override
    public User getUserById(Long id) {
        return userRepository.findById(id).orElse(null);
    }
}
