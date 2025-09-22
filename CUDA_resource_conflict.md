# The Exact Issue : Resource conflict

<img width="654" height="439" alt="Screenshot 2025-09-22 at 2 15 57 PM" src="https://github.com/user-attachments/assets/bbab9f9d-b92c-4386-bffa-6287de098598" />


**Root Cause**: Ollama (an AI language model server) was running in the background and had exclusive access to your CUDA resources, specifically monopolizing the nvidia-uvm (Unified Virtual Memory) subsystem.

**Technical Details**:
- Ollama process (PID 4499) held open file descriptors to all four NVIDIA GPU devices (/dev/nvidia0-3) 
- More critically, it had exclusive access to `/dev/nvidia-uvm` (device 503,0)
- This prevented any other CUDA applications from initializing properly
- Your applications got CUDA error 999 (CUDA_ERROR_UNKNOWN) because the CUDA runtime couldn't access the resources it needed

**Why this happened**: 
- Ollama likely started first and grabbed exclusive CUDA context control
- CUDA's Unified Virtual Memory system doesn't allow sharing when one process has exclusive access
- Your `pkill -f nvidia` and `pkill -f cuda` commands didn't kill Ollama because its process name is just "ollama"

## What I Did to Fix It

**Step 1: Identified the conflicting process**
```bash
sudo lsof /dev/nvidia*
```
This revealed ollama (PID 4499) was holding multiple file descriptors to NVIDIA devices.

**Step 2: Terminated the conflicting process**
```bash
sudo kill 4499
sudo pkill -f ollama
```

**Step 3: Reloaded the nvidia-uvm module**
```bash
sudo rmmod nvidia_uvm #rmmod removes a loaded kernel module from the running Linux kernel, essentially unloading driver code from memory.
sudo modprobe nvidia_uvm # modprobe loads a kernel module into the running Linux kernel, automatically handling any dependencies by loading required modules first.
# Check if device was recreated
ls -la /dev/nvidia-uvm

# Test again
./test_minimal
```
This cleared any corrupted state in the CUDA subsystem and recreated the `/dev/nvidia-uvm` device.

**Step 4: Verified the fix**
- Confirmed no processes were holding `/dev/nvidia-uvm`
- Tested CUDA functionality - all 4 GPUs now accessible

**Step 5: Improved initialization code**
- Identified that your original code used `cudaFree(0)` as an initialization trick
- This can cause issues when the system is in a corrupted state
- Replaced with direct CUDA API calls for more robust initialization

## Prevention for Future

To run both Ollama and your CUDA applications simultaneously:
```bash
# Limit Ollama to specific GPU
CUDA_VISIBLE_DEVICES=0 ollama serve

# Run your applications on other GPUs
CUDA_VISIBLE_DEVICES=1,2,3 ./your_cuda_app
```

The fix was essentially: kill the resource-hogging process, reset the CUDA subsystem, and improve the initialization pattern.
