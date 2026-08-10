package com.its.project.repository;

import org.springframework.data.jpa.repository.JpaRepository;

import com.its.project.model.Project;

public interface ProjectRepository extends JpaRepository<Project, Long> {

}


package com.its.project.service;

import java.util.List;

import com.its.project.model.Project;

public interface ProjectService {

    Project createProject(Project project);

    List<Project> getAllProjects();

    Project getProjectById(Long id);
}


package com.its.project.service;

import java.util.List;

import org.springframework.stereotype.Service;

import com.its.project.model.Project;
import com.its.project.repository.ProjectRepository;

@Service
public class ProjectServiceImpl implements ProjectService {

    private final ProjectRepository projectRepository;

    public ProjectServiceImpl(ProjectRepository projectRepository) {
        this.projectRepository = projectRepository;
    }

    @Override
    public Project createProject(Project project) {
        return projectRepository.save(project);
    }

    @Override
    public List<Project> getAllProjects() {
        return projectRepository.findAll();
    }

    @Override
    public Project getProjectById(Long id) {
        return projectRepository.findById(id).orElse(null);
    }
}



