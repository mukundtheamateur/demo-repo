    @PostMapping("/addEmployee")
    public ResponseEntity<Employee> addEmployee(@Valid @RequestBody Employee employee) {
         log.info("Adding new employee: {}", employee.getName());
        return ResponseEntity.ok(employeeService.addEmployee(employee));
    }

{
    "id": 1,
    "employeeId": 123456,
    "name": "Mukund",
    "age": 23,
    "department": {
        "id": 1,
        "name": "dep1",
        "employees": [
            {
                "id": 1,
                "employeeId": 123456,
                "name": "Mukund",
                "age": 23,
                "department": {
                    "id": 1,
                    "name": "dep1",
                    "employees": [
                        {
                         
}

i'm getting nested output in the response body 

    @GetMapping("/getEmployee/{id}")
    public ResponseEntity<Employee> getEmployee(@PathVariable Integer id) {
        // log.info("Fetching employee with ID: {}", id);
        return ResponseEntity.ok(employeeService.getEmployeeByEmployeeId(id));
    }

    public Employee getEmployeeByEmployeeId(Integer id) {
        if (id < 100000 || id > 999999) {
            throw new IllegalArgumentException("Employee ID must be a 6-digit number.");
        }

        return (Employee) employeeRepository.findByEmployeeId(id)
                .orElseThrow(() -> new EmployeeNotFoundException("Employee not found with ID: " + id));
    }
