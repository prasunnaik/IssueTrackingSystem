package com.its.project.controller;

import java.util.List;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.PutMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import com.its.project.model.Project;
import com.its.project.service.ProjectService;

import jakarta.validation.Valid;

@RestController
@RequestMapping("/api/projects")
public class ProjectController {

    private final ProjectService projectService;

    public ProjectController(ProjectService projectService) {
        this.projectService = projectService;
    }

    @GetMapping
    public ResponseEntity<List<Project>> getAllProjects() {

        return new ResponseEntity<>(
                projectService.getAllProjects(),
                HttpStatus.OK
        );
    }

    @PostMapping
    public ResponseEntity<Project> createProject(
            @Valid @RequestBody Project project) {

        return new ResponseEntity<>(
                projectService.createProject(project),
                HttpStatus.CREATED
        );
    }

    @GetMapping("/{projectId}")
    public ResponseEntity<Project> getProjectById(
            @PathVariable Long projectId) {

        return new ResponseEntity<>(
                projectService.getProjectById(projectId),
                HttpStatus.OK
        );
    }

    @PutMapping("/{projectId}")
    public ResponseEntity<Project> updateProject(
            @PathVariable Long projectId,
            @Valid @RequestBody Project project) {

        return new ResponseEntity<>(
                projectService.updateProject(projectId, project),
                HttpStatus.OK
        );
    }

    @DeleteMapping("/{projectId}")
    public ResponseEntity<Void> deleteProject(
            @PathVariable Long projectId) {

        projectService.deleteProject(projectId);

        return new ResponseEntity<>(
                HttpStatus.NO_CONTENT
        );
    }

    @GetMapping("/owner/{ownerId}")
    public ResponseEntity<List<Project>> getProjectsByOwner(
            @PathVariable Long ownerId) {

        return new ResponseEntity<>(
                projectService.getProjectsByOwner(ownerId),
                HttpStatus.OK
        );
    }
}

package com.its.project.service;

import java.util.List;

import com.its.project.model.Project;

public interface ProjectService {

    Project createProject(Project project);

    List<Project> getAllProjects();

    Project getProjectById(Long projectId);

    Project updateProject(Long projectId, Project project);

    void deleteProject(Long projectId);

    List<Project> getProjectsByOwner(Long ownerId);
}


package com.its.project.service;

import java.util.List;

import org.springframework.stereotype.Service;

import com.its.project.exception.ProjectNotFoundException;
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
    public Project getProjectById(Long projectId) {

        return projectRepository.findById(projectId)
                .orElseThrow(() ->
                    new ProjectNotFoundException(
                        "Project not found with id: " + projectId
                    )
                );
    }

    @Override
    public Project updateProject(Long projectId, Project project) {

        Project existingProject = projectRepository.findById(projectId)
                .orElseThrow(() ->
                    new ProjectNotFoundException(
                        "Project not found with id: " + projectId
                    )
                );

        existingProject.setProjectName(project.getProjectName());
        existingProject.setProductOwnerId(project.getProductOwnerId());
        existingProject.setStartDate(project.getStartDate());
        existingProject.setEndDate(project.getEndDate());

        return projectRepository.save(existingProject);
    }

    @Override
    public void deleteProject(Long projectId) {

        Project existingProject = projectRepository.findById(projectId)
                .orElseThrow(() ->
                    new ProjectNotFoundException(
                        "Project not found with id: " + projectId
                    )
                );

        projectRepository.delete(existingProject);
    }

    @Override
    public List<Project> getProjectsByOwner(Long ownerId) {
        return projectRepository.findByProductOwnerId(ownerId);
    }
}
