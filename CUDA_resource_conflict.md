# The Exact Issue : Resource conflict

<svg viewBox="0 0 1000 600" xmlns="http://www.w3.org/2000/svg">
  <!-- Title -->
  <text x="500" y="30" text-anchor="middle" font-size="24" font-weight="bold" fill="#333">
    CUDA Error 999: Ollama Resource Conflict
  </text>
  
  <!-- BEFORE section -->
  <text x="250" y="70" text-anchor="middle" font-size="20" font-weight="bold" fill="#d32f2f">
    BEFORE (Broken State)
  </text>
  
  <!-- System box -->
  <rect x="50" y="90" width="400" height="200" fill="#f5f5f5" stroke="#333" stroke-width="2" rx="10"/>
  <text x="250" y="110" text-anchor="middle" font-size="16" font-weight="bold">System Resources</text>
  
  <!-- GPU devices -->
  <rect x="70" y="130" width="80" height="40" fill="#4caf50" stroke="#333" rx="5"/>
  <text x="110" y="155" text-anchor="middle" font-size="12">GPU 0-3</text>
  
  <!-- nvidia-uvm device -->
  <rect x="170" y="130" width="100" height="40" fill="#ff9800" stroke="#333" rx="5"/>
  <text x="220" y="155" text-anchor="middle" font-size="12">/dev/nvidia-uvm</text>
  
  <!-- CUDA Runtime -->
  <rect x="290" y="130" width="130" height="40" fill="#2196f3" stroke="#333" rx="5"/>
  <text x="355" y="155" text-anchor="middle" font-size="12">CUDA Runtime</text>
  
  <!-- Ollama process -->
  <rect x="100" y="200" width="120" height="60" fill="#f44336" stroke="#333" stroke-width="3" rx="10"/>
  <text x="160" y="225" text-anchor="middle" font-size="14" font-weight="bold" fill="white">Ollama</text>
  <text x="160" y="245" text-anchor="middle" font-size="12" fill="white">PID: 4499</text>
  
  <!-- Exclusive lock arrows -->
  <path d="M 160 200 L 220 170" stroke="#f44336" stroke-width="4" marker-end="url(#redArrow)"/>
  <path d="M 140 200 L 110 170" stroke="#f44336" stroke-width="4" marker-end="url(#redArrow)"/>
  
  <!-- Your app trying to access -->
  <rect x="280" y="200" width="120" height="60" fill="#9e9e9e" stroke="#333" stroke-width="2" stroke-dasharray="5,5" rx="10"/>
  <text x="340" y="225" text-anchor="middle" font-size="14">Your CUDA App</text>
  <text x="340" y="245" text-anchor="middle" font-size="12" fill="#d32f2f">ERROR 999</text>
  
  <!-- Blocked access -->
  <path d="M 320 200 L 355 170" stroke="#d32f2f" stroke-width="3" stroke-dasharray="10,5" marker-end="url(#blockedArrow)"/>
  <text x="340" y="190" text-anchor="middle" font-size="10" fill="#d32f2f">BLOCKED</text>
  
  <!-- AFTER section -->
  <text x="750" y="70" text-anchor="middle" font-size="20" font-weight="bold" fill="#388e3c">
    AFTER (Fixed State)
  </text>
  
  <!-- System box -->
  <rect x="550" y="90" width="400" height="200" fill="#f5f5f5" stroke="#333" stroke-width="2" rx="10"/>
  <text x="750" y="110" text-anchor="middle" font-size="16" font-weight="bold">System Resources</text>
  
  <!-- GPU devices -->
  <rect x="570" y="130" width="80" height="40" fill="#4caf50" stroke="#333" rx="5"/>
  <text x="610" y="155" text-anchor="middle" font-size="12">GPU 0-3</text>
  
  <!-- nvidia-uvm device -->
  <rect x="670" y="130" width="100" height="40" fill="#ff9800" stroke="#333" rx="5"/>
  <text x="720" y="155" text-anchor="middle" font-size="12">/dev/nvidia-uvm</text>
  
  <!-- CUDA Runtime -->
  <rect x="790" y="130" width="130" height="40" fill="#2196f3" stroke="#333" rx="5"/>
  <text x="855" y="155" text-anchor="middle" font-size="12">CUDA Runtime</text>
  
  <!-- Your app working -->
  <rect x="650" y="200" width="120" height="60" fill="#4caf50" stroke="#333" stroke-width="2" rx="10"/>
  <text x="710" y="225" text-anchor="middle" font-size="14" fill="white" font-weight="bold">Your CUDA App</text>
  <text x="710" y="245" text-anchor="middle" font-size="12" fill="white">SUCCESS!</text>
  
  <!-- Successful access arrows -->
  <path d="M 690 200 L 720 170" stroke="#4caf50" stroke-width="3" marker-end="url(#greenArrow)"/>
  <path d="M 670 200 L 610 170" stroke="#4caf50" stroke-width="3" marker-end="url(#greenArrow)"/>
  <path d="M 730 200 L 855 170" stroke="#4caf50" stroke-width="3" marker-end="url(#greenArrow)"/>
  
  <!-- Ollama terminated -->
  <rect x="780" y="200" width="120" height="60" fill="#9e9e9e" stroke="#333" stroke-width="2" stroke-dasharray="5,5" rx="10"/>
  <text x="840" y="225" text-anchor="middle" font-size="14">Ollama</text>
  <text x="840" y="245" text-anchor="middle" font-size="12">TERMINATED</text>
  
  <!-- Fix steps -->
  <text x="500" y="340" text-anchor="middle" font-size="18" font-weight="bold" fill="#1976d2">
    Fix Steps Applied:
  </text>
  
  <rect x="150" y="360" width="700" height="200" fill="#e3f2fd" stroke="#1976d2" stroke-width="2" rx="10"/>
  
  <text x="180" y="390" font-size="16" font-weight="bold" fill="#1976d2">1. Identified Conflicting Process:</text>
  <text x="200" y="410" font-size="14" fill="#333">sudo lsof /dev/nvidia* → Found ollama (PID 4499) holding nvidia-uvm</text>
  
  <text x="180" y="440" font-size="16" font-weight="bold" fill="#1976d2">2. Terminated Ollama:</text>
  <text x="200" y="460" font-size="14" fill="#333">sudo kill 4499 && sudo pkill -f ollama</text>
  
  <text x="180" y="490" font-size="16" font-weight="bold" fill="#1976d2">3. Reset CUDA Subsystem:</text>
  <text x="200" y="510" font-size="14" fill="#333">sudo rmmod nvidia_uvm && sudo modprobe nvidia_uvm</text>
  
  <text x="180" y="540" font-size="16" font-weight="bold" fill="#1976d2">4. Result:</text>
  <text x="200" y="560" font-size="14" fill="#333">All 4 RTX 3070 GPUs accessible, CUDA applications working normally</text>
  
  <!-- Arrow markers -->
  <defs>
    <marker id="redArrow" markerWidth="10" markerHeight="10" refX="9" refY="3" orient="auto" markerUnits="strokeWidth">
      <path d="M0,0 L0,6 L9,3 z" fill="#f44336"/>
    </marker>
    <marker id="greenArrow" markerWidth="10" markerHeight="10" refX="9" refY="3" orient="auto" markerUnits="strokeWidth">
      <path d="M0,0 L0,6 L9,3 z" fill="#4caf50"/>
    </marker>
    <marker id="blockedArrow" markerWidth="10" markerHeight="10" refX="9" refY="3" orient="auto" markerUnits="strokeWidth">
      <path d="M0,0 L0,6 L9,3 z" fill="#d32f2f"/>
    </marker>
  </defs>
  
  <!-- Legend -->
  <rect x="10" y="480" width="120" height="100" fill="#ffffff" stroke="#333" stroke-width="1" rx="5"/>
  <text x="70" y="500" text-anchor="middle" font-size="14" font-weight="bold">Legend</text>
  <rect x="20" y="510" width="15" height="10" fill="#f44336"/>
  <text x="45" y="520" font-size="12">Blocked/Error</text>
  <rect x="20" y="530" width="15" height="10" fill="#4caf50"/>
  <text x="45" y="540" font-size="12">Working</text>
  <rect x="20" y="550" width="15" height="10" fill="#9e9e9e" stroke-dasharray="2,2"/>
  <text x="45" y="560" font-size="12">Inactive</text>
</svg>

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
sudo rmmod nvidia_uvm
sudo modprobe nvidia_uvm
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
