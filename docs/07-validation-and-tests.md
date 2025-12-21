# End-to-End Validation

This section summarizes the key tests that prove the entire lab design works as intended.

1. **Name resolution & internal access**
   - From `az-adminvm-lab`, run:
     - `nslookup az-appvm-lab.az.pdns.lab`
     - `ssh testuser@az-appvm-lab.az.pdns.lab`
   - Expect: FQDN resolves to `10.0.3.4` and SSH login succeeds.

2. **No direct internet access to app VM**
   - Confirm `az-appvm-lab` has **no public IP** assigned in the portal.
   - Attempt to SSH directly from your home machine:
     - `ssh testuser@<any previous public IP>` → fails.

3. **Outbound forced through Firewall**
   - App NIC Effective routes show `0.0.0.0/0` → Virtual appliance `10.0.1.4`.
   - From `az-appvm-lab`:
     - `curl -I https://github.com` → **200**
     - `curl -I https://archive.ubuntu.com` → **200**
     - `curl -I https://google.com` → **fails**

4. **Segmentation rules**
   - Verify NSG on mgmt subnet only allows SSH from approved admin IPs.
   - Verify NSG on app subnet only allows SSH from mgmt subnet.

For detailed screenshots and configuration steps, see the component SOPs in `/docs`.
