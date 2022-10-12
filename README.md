# CRUD_practice

## CRUD 연습
> * POSTMAN 사용
> * JSON 형식 데이터 CRUD 가능

### POST
> ![crud_post](https://user-images.githubusercontent.com/105911312/195015745-da5cff3b-6cec-40c6-a793-5b795f04c26d.png)
```
  @PostMapping
    public ResponseEntity<TodoResponse> create(@RequestBody TodoRequest request) {
        System.out.println("CREATE");

        if (ObjectUtils.isEmpty(request.getTitle()))
            return ResponseEntity.badRequest().build();

        if (ObjectUtils.isEmpty(request.getOrder()))
            request.setOrder(0L);

        if (ObjectUtils.isEmpty(request.getCompleted()))
            request.setCompleted(false);

        TodoEntity result = this.service.add(request);
        return ResponseEntity.ok(new TodoResponse(result));
    }
```
### GET
> ![crud_get](https://user-images.githubusercontent.com/105911312/195015698-cb9d0853-7464-46e0-b92c-8d23fb8a8f35.png)
```
@GetMapping
    public ResponseEntity<List<TodoResponse>> readAll() {
        System.out.println("READ ALL");
        List<TodoEntity> list = this.service.searchAll();
        List<TodoResponse> response = list.stream().map(TodoResponse::new)
                .collect(Collectors.toList());
        return ResponseEntity.ok(response);
    }
```

### DELETE
> ![crud_del](https://user-images.githubusercontent.com/105911312/195015815-5bb18a17-eabe-4ede-800a-1604a4ad8903.png)
```
@DeleteMapping("{id}")
    public ResponseEntity<?> deleteOne(@PathVariable Long id) {
        System.out.println("DELETE ONE");
        this.service.deleteById(id);
        return  ResponseEntity.ok().build();
    }
```

### after_DELETE / GET
> ![crud_afterdel_get](https://user-images.githubusercontent.com/105911312/195015896-dd8b122d-44f3-44a8-be96-331ebdfee1d5.png)

### UPDATE
![crud_beforepatch](https://user-images.githubusercontent.com/105911312/195017438-a5b2a69e-57f6-44db-8671-7e2e1b61353e.png)
![crud_afterpatch](https://user-images.githubusercontent.com/105911312/195017478-97492fd5-8d33-4386-80a3-413b3e4a114d.png)
```
@PatchMapping("{id}")
    public ResponseEntity<TodoEntity> update(@PathVariable Long id, @RequestBody TodoRequest request) {
        System.out.println("UPDATE");
        TodoEntity result = this.service.updateById(id, request);
        return ResponseEntity.ok(result);
    }
```

### after_UPDATE
![crud_afterpatch_finish](https://user-images.githubusercontent.com/105911312/195017504-597345fc-21f3-430e-82c3-c7f50401c96d.png)
