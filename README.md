# Things-Kit gRPC

**gRPC Server Module for Things-Kit**

Lifecycle-managed gRPC server with automatic service registration and graceful shutdown.

## Installation

```bash
go get github.com/things-kit/things-kit-grpc
```

## Features

- Automatic gRPC server lifecycle management
- Service registration via dependency injection
- Configuration via Viper or environment variables
- Graceful startup and shutdown
- Zero-boilerplate service binding

## Quick Start

```go
package main

import (
    "github.com/things-kit/things-kit/app"
    "github.com/things-kit/things-kit/logging"
    "github.com/things-kit/things-kit/viperconfig"
    grpcmodule "github.com/things-kit/things-kit-grpc"
    pb "your/protobuf/package"
)

func main() {
    app.New(
        viperconfig.Module,
        logging.Module,
        grpcmodule.Module,
        grpcmodule.AsGrpcService(NewUserService, pb.RegisterUserServiceServer),
    ).Run()
}

type UserService struct {
    pb.UnimplementedUserServiceServer
}

func NewUserService() *UserService {
    return &UserService{}
}

func (s *UserService) GetUser(ctx context.Context, req *pb.GetUserRequest) (*pb.UserResponse, error) {
    // implementation
    return &pb.UserResponse{}, nil
}
```

## Configuration

Via `config.yaml`:
```yaml
grpc:
  port: 50051
```

Or environment variable:
```bash
export GRPC_PORT=50051
```

## Service Registration

Use `AsGrpcService` to register services:

```go
grpcmodule.AsGrpcService(
    NewYourService,              // Constructor function
    pb.RegisterYourServiceServer, // gRPC registrar function
)
```

The constructor can accept any dependencies via Fx injection.

## License

MIT License - see LICENSE file for details
