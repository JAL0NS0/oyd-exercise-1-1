## Task 1 — Initialize and validate

**terraform init**

```chl
Initializing the backend...
Initializing provider plugins...
- Finding hashicorp/aws versions matching "~> 5.0"...
- Installing hashicorp/aws v5.100.0...
- Installed hashicorp/aws v5.100.0 (signed by HashiCorp)
Terraform has created a lock file .terraform.lock.hcl to record the provider
selections it made above. Include this file in your version control repository
so that Terraform can guarantee to make the same selections by default when
you run "terraform init" in the future.

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.
```

**terraform validate**

```chl
Success! The configuration is valid.
```

## Task 2 — Read the plan

**terraform plan**

```chl
Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following
symbols:
  + create

Terraform will perform the following actions:

  # aws_s3_bucket.exercise will be created
  + resource "aws_s3_bucket" "exercise" {
      + acceleration_status         = (known after apply)
      + acl                         = (known after apply)
      + arn                         = (known after apply)
      + bucket                      = "oyd-exercise-bucket-2026"
      + bucket_domain_name          = (known after apply)
      + bucket_prefix               = (known after apply)
      + bucket_regional_domain_name = (known after apply)
      + force_destroy               = false
      + hosted_zone_id              = (known after apply)
      + id                          = (known after apply)
      + object_lock_enabled         = (known after apply)
      + policy                      = (known after apply)
      + region                      = (known after apply)
      + request_payer               = (known after apply)
      + tags                        = {
          + "Environment" = "dev"
          + "ManagedBy"   = "terraform"
        }
      + tags_all                    = {
          + "Environment" = "dev"
          + "ManagedBy"   = "terraform"
        }
      + website_domain              = (known after apply)
      + website_endpoint            = (known after apply)

      + cors_rule (known after apply)

      + grant (known after apply)

      + lifecycle_rule (known after apply)

      + logging (known after apply)

      + object_lock_configuration (known after apply)

      + replication_configuration (known after apply)

      + server_side_encryption_configuration (known after apply)

      + versioning (known after apply)

      + website (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.

─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

Note: You didn't use the -out option to save this plan, so Terraform can't guarantee to take exactly these actions if you run
"terraform apply" now.
```


1. How many resources will be created, changed, or destroyed?

> - 1 creado
> - 0 cambios
> - 0 destruidos

2. What does the + symbol next to each attribute mean?

> Que será creado cuando se ejecute

3. Find one attribute in the plan marked as (known after apply). Why can't Terraform know that value before apply?

```chl
+ arn    = (known after apply)
```

> Es un código de identificación del recurso que se le asigna hasta el momento de crearse.

## Task 3 — Predict a change

**terraform plan**

```chl
# aws_s3_bucket.exercise will be created
  + resource "aws_s3_bucket" "exercise" {
      + acceleration_status         = (known after apply)
      + acl                         = (known after apply)
      + arn                         = (known after apply)
      + bucket                      = "oyd-exercise-bucket-2026"
      + bucket_domain_name          = (known after apply)
      + bucket_prefix               = (known after apply)
      + bucket_regional_domain_name = (known after apply)
      + force_destroy               = false
      + hosted_zone_id              = (known after apply)
      + id                          = (known after apply)
      + object_lock_enabled         = (known after apply)
      + policy                      = (known after apply)
      + region                      = (known after apply)
      + request_payer               = (known after apply)
      + tags                        = {
          + "Environment" = "dev"
          + "ManagedBy"   = "terraform"
          + "Owner"       = "Joaquin Marroquin"
        }
      + tags_all                    = {
          + "Environment" = "dev"
          + "ManagedBy"   = "terraform"
          + "Owner"       = "Joaquin Marroquin"
        }
      + website_domain              = (known after apply)
      + website_endpoint            = (known after apply)

      + cors_rule (known after apply)

      + grant (known after apply)

      + lifecycle_rule (known after apply)

      + logging (known after apply)

      + object_lock_configuration (known after apply)

      + replication_configuration (known after apply)

      + server_side_encryption_configuration (known after apply)

      + versioning (known after apply)

      + website (known after apply)
    }
```

1. Did Terraform propose to destroy and recreate the bucket, or update it in place?

> Como no hay nada creado aún, el plan solo coloca que es necesario crearlo con los nuevos cambios implementados.
> Una vez creado creo que propondría solo actualizarlo, ya que una etiqueta no es un dato fundamental del bucket.

2. Why does that distinction matter?

> Se pierden todo lo que esté dentro del bucket si no tiene respaldo.
> Tambien es más tardado y costoso eliminarlo y volverlo a crear que solamente modificarlo.

## Task 4 — State reasoning

1. What does the absence (or presence) of the state file tell you about the relationship between plan and state?

> No existía, como no se ha ejecutado creo que el state es el estado actual del bucket en aws, como este solamente fue un plan aun  no hay objetos en el bucket y no hay punto de comparación al ver el plan.