package com.its.user.repository;

import org.springframework.data.jpa.repository.JpaRepository;

import com.its.user.model.User;

public interface UserRepository extends JpaRepository<User, Long> {

    User findByEmail(String email);
}
