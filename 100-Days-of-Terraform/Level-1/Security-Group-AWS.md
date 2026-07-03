# Security Group in AWS

```
resource "aws_security_group" "datacenter_sg" {
  name        = "datacenter-sg"
  description = "Security group for Nautilus App Servers"

  // Inbound rules
  ingress {
    from_port   = 80                     # Port range for the rule
    to_port     = 80                     # Port range for the rule
    protocol    = "tcp"                  # Protocol (tcp, udp, icmp)
    cidr_blocks = ["0.0.0.0/0"]          # Allowed IP address range (0.0.0.0/0 allows all)
  }

  ingress {
    from_port   = 22                     # Port range for the rule
    to_port     = 22                     # Port range for the rule
    protocol    = "tcp"                  # Protocol
    cidr_blocks = ["0.0.0.0/0"]     # Replace with your IP or CIDR range
  }

  // Outbound rules
  egress {
    from_port   = 0                       # Port range for all outbound traffic
    to_port     = 0                       # Port range for all outbound traffic
    protocol    = "-1"                    # All protocols
    cidr_blocks = ["0.0.0.0/0"]           # Allowed IP address range for outbound traffic
  }
}
```
