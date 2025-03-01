# demo-repo[updated by local repo]

hello I have edited this in cloud


package com.cts.assessment.controller;

import com.cts.assessment.entity.Employee;
import com.cts.assessment.service.EmployeeService;
import jakarta.validation.Valid;
import lombok.RequiredArgsConstructor;
import lombok.extern.slf4j.Slf4j;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/employee")
@RequiredArgsConstructor
@Slf4j
public class EmployeeController {

    private final EmployeeService employeeService;

    @GetMapping("/getEmployee/{id}")
    public ResponseEntity<Employee> getEmployee(@PathVariable Long id) {
        log.info("Fetching employee with ID: {}", id);
        return ResponseEntity.ok(employeeService.getEmployeeById(id));
    }

    @PostMapping("/getEmployeeByDepartmentId")
    public ResponseEntity<List<Employee>> getEmployeeByDepartment(@RequestBody Long departmentId) {
        log.info("Fetching employees for department ID: {}", departmentId);
        return ResponseEntity.ok(employeeService.getEmployeesByDepartmentId(departmentId));
    }

    @PostMapping("/addEmployee")
    public ResponseEntity<Employee> addEmployee(@Valid @RequestBody Employee employee) {
        log.info("Adding new employee: {}", employee.getName());
        return ResponseEntity.ok(employeeService.addEmployee(employee));
    }

    @PutMapping("/editEmployee")
    public ResponseEntity<Employee> editEmployee(@Valid @RequestBody Employee employee) {
        log.info("Editing employee: {}", employee.getId());
        return ResponseEntity.ok(employeeService.editEmployee(employee));
    }

    @DeleteMapping("/deleteEmployee/{id}")
    public ResponseEntity<String> deleteEmployee(@PathVariable Long id) {
        log.info("Deleting employee with ID: {}", id);
        employeeService.deleteEmployee(id);
        return ResponseEntity.ok("Employee deleted successfully.");
    }
}
