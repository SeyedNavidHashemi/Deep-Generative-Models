# Deep-Generative-Models
A collection of computer assignments for the Deep Generative Models course.
README
Distributed Calculator Using Named Pipes (Go)

This project implements a simple distributed arithmetic system using two separate Go programs:

    worker.go — the server/worker process
    interface.go — the client interface that sends requests and receives responses

Both programs communicate via Linux named pipes (FIFOs) using a simple line-based request protocol and JSON‑formatted responses.
1. How to Run the Worker

The Worker must always be started before the Interface.

                                                                    bash
go run worker.go

When executed, the Worker:

    Removes any old pipes (/tmp/dist_req.pipe and /tmp/dist_res.pipe)
    Creates fresh named pipes
    Listens for incoming requests
    Computes the requested operation
    Returns JSON responses
    Cleans up pipes again on exit (Ctrl+C)

The Worker must remain running while the Interface is being used.
2. How to Run the Interface

Run the Interface in a separate terminal while the Worker is active:

                                                                    bash
go run interface.go

If the Worker is not running, or the named pipes do not exist, the Interface prints:

                                                                    text
ERR worker_not_running: worker process is not running, please run it first

Once the Worker is active, the Interface accepts commands interactively:

                                                                    text
> ADD 3 5
> SUB 10 4
> POW 2 8

3. Correct Execution Order

The correct runtime sequence is:

    Open Terminal #1Start the Worker:

                                                                    bash
   go run worker.go

    Open Terminal #2Start the Interface:

                                                                    bash
   go run interface.go

    Enter commands in the Interface (Terminal #2)
    To stop:
        Press Ctrl+C in the Worker terminal (Terminal #1)
        This automatically deletes the named pipes

If the Interface is run before the Worker, it immediately exits with a worker_not_running error.
4. Example Inputs and Outputs

Below are real examples that demonstrate correct behavior of the system.
Valid Commands

                                                                    text
> ADD 4 5
OK 9

> SUB 10 3
OK 7

> MUL 6 7
OK 42

> DIV 12 3
OK 4

> POW 2 8
OK 256

Invalid Commands

Unknown command:

                                                                    text
> HELLO 1 2
ERR unknown_command: unsupported operation

Argument count error:

                                                                    text
> ADD 5
ERR invalid_argument_count: expected exactly two arguments

Non-numeric argument:

                                                                    text
> ADD F 4
ERR non_numeric_argument: arguments must be integers

Division by zero:

                                                                    text
> DIV 10 0
ERR division_by_zero: cannot divide by zero

Worker crash or closed:

                                                                    text
ERR pipe_read_error: connection lost or worker closed unexpectedly

5. Short Description of the Message Exchange Protocol

The system uses two named pipes:

    Request pipe: /tmp/dist_req.pipeUsed by the Interface to send commands to the Worker
    Response pipe: /tmp/dist_res.pipeUsed by the Worker to return JSON results

Request Format

The Interface sends one line per request, using:

                                                                    text
COMMAND operand1 operand2

Where COMMAND ∈ { ADD, SUB, MUL, DIV, POW }

Example:

                                                                    text
ADD 3 7

Response Format

The Worker always responds with a single JSON object, either:

Success:

                                                                    json
{"status":"OK","result":10}

Error:

                                                                    json
{"status":"ERR","error":"division_by_zero"}

Error Handling

The following errors are supported:

    unknown_command
    invalid_argument_count
    non_numeric_argument
    division_by_zero
    worker_not_running
    pipe_write_error
    pipe_read_error
    invalid_response_format

The Interface validates user input before sending requests.

The Worker validates after receiving requests.
Summary

This project demonstrates:

    Creation and clean-up of named pipes (FIFOs)
    Bi-directional IPC using line-based messages
    JSON serialization/deserialization
    Robust input validation
    Graceful handling of Worker disconnects

The system behaves reliably even in edge cases such as malformed input, missing pipes, and unexpected Worker termination
