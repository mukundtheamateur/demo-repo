2025-03-05T19:06:25.133+05:30  WARN 19596 --- [assessment] [nio-8080-exec-2] .w.s.m.s.DefaultHandlerExceptionResolver : Resolved [org.springframework.web.bind.MissingPathVariableException: Required URI template variable 'employeeId' for method parameter type Integer is not present]

@DeleteMapping("/deleteEmployee/{id}")
    public ResponseEntity<String> deleteEmployee(@PathVariable Integer employeeId) {
         log.info("Deleting employee with ID: {}", employeeId);
        employeeService.deleteEmployee(employeeId);
        return ResponseEntity.ok("Employee deleted successfully.");
    }
public void deleteEmployee(Integer employeeId) {
        if (employeeRepository.findByEmployeeId(employeeId).isEmpty()) {
            throw new EmployeeNotFoundException("Employee not found with ID: " + employeeId);
        }
        employeeRepository.deleteById(employeeId);
    }
