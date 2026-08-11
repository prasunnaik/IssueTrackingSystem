package com.its.user.client;

import java.util.List;
import java.util.Map;

import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;

@FeignClient(name = "issue-service")
public interface IssueClient {

    @GetMapping("/api/issues/assignee/{assigneeId}")
    List<Map<String, Object>> getIssuesByAssignee(
            @PathVariable("assigneeId") Long assigneeId);
}
