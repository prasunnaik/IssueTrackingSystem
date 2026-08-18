package com.its.project.controller;

import java.util.List;
import java.util.Map;

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

    // Inter-service communication by project ID
    @GetMapping("/{projectId}/issues")
    public ResponseEntity<List<Map<String, Object>>> getIssuesByProjectId(
            @PathVariable Long projectId) {

        return new ResponseEntity<>(
                projectService.getIssuesByProjectId(projectId),
                HttpStatus.OK
        );
    }

    // Inter-service communication by project name
    @GetMapping("/projectName/{projectName}/issues")
    public ResponseEntity<List<Map<String, Object>>> getIssuesByProjectName(
            @PathVariable String projectName) {

        return new ResponseEntity<>(
                projectService.getIssuesByProjectName(projectName),
                HttpStatus.OK
        );
    }
}

package com.its.project.model;

import java.time.LocalDate;

import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;
import jakarta.persistence.Table;
import jakarta.validation.constraints.NotBlank;
import jakarta.validation.constraints.NotNull;

@Entity
@Table(name = "projects")
public class Project {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @NotBlank(message = "Project name is required")
    private String projectName;

    @NotNull(message = "Product owner ID is required")
    private Long productOwnerId;

    @NotNull(message = "Start date is required")
    private LocalDate startDate;

    @NotNull(message = "End date is required")
    private LocalDate endDate;

    public Project() {
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getProjectName() {
        return projectName;
    }

    public void setProjectName(String projectName) {
        this.projectName = projectName;
    }

    public Long getProductOwnerId() {
        return productOwnerId;
    }

    public void setProductOwnerId(Long productOwnerId) {
        this.productOwnerId = productOwnerId;
    }

    public LocalDate getStartDate() {
        return startDate;
    }

    public void setStartDate(LocalDate startDate) {
        this.startDate = startDate;
    }

    public LocalDate getEndDate() {
        return endDate;
    }

    public void setEndDate(LocalDate endDate) {
        this.endDate = endDate;
    }
}


package com.its.project.repository;

import java.util.List;

import org.springframework.data.jpa.repository.JpaRepository;

import com.its.project.model.Project;

public interface ProjectRepository extends JpaRepository<Project, Long> {

    List<Project> findByProductOwnerId(Long ownerId);

    Project findByProjectName(String projectName);
}
package com.its.project.service;

import java.util.List;
import java.util.Map;

import com.its.project.model.Project;

public interface ProjectService {

    Project createProject(Project project);

    List<Project> getAllProjects();

    Project getProjectById(Long projectId);

    Project updateProject(Long projectId, Project project);

    void deleteProject(Long projectId);

    List<Project> getProjectsByOwner(Long ownerId);

    List<Map<String, Object>> getIssuesByProjectId(Long projectId);

    List<Map<String, Object>> getIssuesByProjectName(String projectName);
}
package com.its.project.service;

import java.util.List;
import java.util.Map;

import org.springframework.stereotype.Service;

import com.its.project.client.IssueClient;
import com.its.project.exception.ProjectNotFoundException;
import com.its.project.model.Project;
import com.its.project.repository.ProjectRepository;

@Service
public class ProjectServiceImpl implements ProjectService {

    private final ProjectRepository projectRepository;
    private final IssueClient issueClient;

    public ProjectServiceImpl(
            ProjectRepository projectRepository,
            IssueClient issueClient) {

        this.projectRepository = projectRepository;
        this.issueClient = issueClient;
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

    // Inter-service communication using project ID
    @Override
    public List<Map<String, Object>> getIssuesByProjectId(Long projectId) {

        // Verify project exists first
        getProjectById(projectId);

        return issueClient.getIssuesByProject(projectId);
    }

    // Inter-service communication using project name
    @Override
    public List<Map<String, Object>> getIssuesByProjectName(
            String projectName) {

        Project project = projectRepository.findByProjectName(projectName);

        if (project == null) {
            throw new ProjectNotFoundException(
                "Project not found with name: " + projectName
            );
        }

        return issueClient.getIssuesByProject(project.getId());
    }
}
