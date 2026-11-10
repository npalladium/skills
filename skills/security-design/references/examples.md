# Security Design Examples

These abstract examples calibrate the workflow; they are not exhaustive vulnerability patterns.

## Caller authority versus service authority

**Unsafe candidate:** An endpoint accepts a caller-chosen document ID. A shared service identity reads that document, and the endpoint returns its contents. The caller has permission to invoke the endpoint but no access to the document. If no control binds the original caller's document permission to that read, the service may be acting as a confused deputy. Confirm that the caller cannot already read the data and that the response exposes it.

**Safe contrast:** The service fetches the document using the caller's delegated identity, or checks the caller's access to that exact document before returning it. A service identity having broad access, by itself, does not establish a flaw.

## Binding an execution identity versus using one

**Unsafe candidate:** A caller can choose a customer-managed identity for a new or updated workload, but the binding operation does not check permission to attach or delegate that identity. The workload then runs code under it. Check authorization at the binding/activation point and demonstrate the resulting authority.

**Safe contrast:** The caller already has permission to bind the identity and can inspect and revoke its downstream grants. Merely invoking a workload that an authorized administrator already bound is not evidence that the binding gate is missing.

## Network reachability is authority

**Unsafe candidate:** A shared managed service accepts a caller-chosen private-network attachment and can reach internal hosts there, but checks only the caller's permission to invoke the service. A target-owner allowlist admits the shared service fleet but cannot distinguish the caller's tenant. Establish whether the caller can use that network resource and whether a private effect or observation is reachable.

**Safe contrast:** The service checks the caller's use/ownership of the specific attachment, uses a tenant-scoped connector, or can only reach networks the caller already controls. A request being accepted is not proof of private reachability.

## Source authorization versus derived data

**Unsafe candidate:** A service ingests records with per-record access controls into a shared index, then serves search results using only coarse index access. A reader denied the source record can retrieve its protected content from the derivative.

**Safe contrast:** Retrieval rechecks the source policy or enforces an equivalent per-record policy copied and kept current in the index. Customer-consented sharing of curated output is not automatically an authorization flaw.

## Negative calibration

A capable feature is not inherently a flaw. A security finding needs a specific guarantee, a missing or bypassed control, a principal/resource path, and an effect beyond the caller's baseline. Where authorization is visible, deliberately granted, scoped, and revocable, investigate whether the behavior is intended before labeling it insecure by design.
