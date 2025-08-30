# ZMK Firmware Setup Plan for CRKBD v4.2

## Hardware Overview

**Your Setup:**
- **Keyboard**: CRKBD (Corne) v4.2 - 42-key split keyboard
- **MCU**: nice!nano v2 with nRF52840 (Board-ID: nRF52840-nicenano)
- **Displays**: nice!view e-paper displays (128x32)
- **RGB Lighting**: RGB underglow with addressable LEDs
- **Encoders**: Rotary encoders for volume/navigation control
- **Joystick**: Analog joystick for mouse emulation/navigation
- **Connection**: Wireless via Bluetooth
- **Layout Goal**: Colemak-DH
- **Bootloader**: UF2 Bootloader 0.6.0 (confirmed working)

## Phase 1: Understanding Your Hardware Setup 🔍

### Key Concepts to Grasp:
- **Split Architecture**: Two independent halves communicating wirelessly
- **Connection Flow**: Right half → Left half → Computer (via Bluetooth)
- **Power Management**: Each half has its own battery and power management
- **Display Integration**: nice!view shows real-time keyboard status

### Learning Objectives:
- [ ] Understand split keyboard communication
- [ ] Learn about ZMK's wireless architecture
- [ ] Familiarize with nice!nano pinout and capabilities
- [ ] Understand nice!view display integration

### Resources:
- [ZMK Split Keyboard Documentation](https://zmk.dev/docs/features/split-keyboards)
- [nice!nano Documentation](https://nicekeyboards.com/docs/nice-nano/)

## Phase 2: ZMK Configuration Repository Setup 📁

### Repository Structure:
```
zmk-config/
├── .github/
│   └── workflows/
│       └── build.yml          # Automated build configuration
├── config/
│   ├── corne.keymap          # Your keymap definition
│   ├── corne.conf            # Hardware configuration
│   └── west.yml              # ZMK version pinning
├── build.yaml                # Build targets definition
└── README.md                 # Documentation
```

### Key Files to Understand:

#### `build.yaml`
Defines what firmware gets built:
```yaml
include:
  - board: nice_nano_v2
    shield: corne_left nice_view_adapter nice_view
  - board: nice_nano_v2
    shield: corne_right nice_view_adapter nice_view
```

#### `config/corne.conf`
Hardware-specific settings:
- Display configuration
- Bluetooth settings
- Power management
- Feature enables/disables

#### `config/corne.keymap`
Your actual key layout and behaviors

### Learning Objectives:
- [ ] Set up GitHub repository
- [ ] Understand build system
- [ ] Configure automated builds
- [ ] Learn configuration file hierarchy

### Tasks:
1. Fork ZMK config template or create new repository
2. Configure build.yaml for your hardware
3. Set up GitHub Actions
4. Test initial build

## Phase 3: Basic Keymap Configuration ⌨️

### Incremental Approach:

#### Step 3.1: Start with QWERTY
- Basic 3-layer layout (base, lower, raise)
- Essential keys only
- Test all physical keys work

#### Step 3.2: Add Essential Behaviors
- Layer switching (momentary and toggle)
- Basic modifiers (Ctrl, Alt, Shift, GUI)
- Essential symbols and numbers

#### Step 3.3: Convert to Colemak-DH
- Gradually transition from QWERTY
- Maintain muscle memory with familiar modifier positions
- Test typing comfort and accuracy

### Key Concepts:
- **Layers**: Different key functions on same physical keys
- **Behaviors**: How keys respond (tap, hold, combinations)
- **Keycodes**: ZMK's key definitions
- **Hold-Tap**: Keys that do different things on tap vs hold

### Colemak-DH Layout Reference:
```
Q W F P B   J L U Y ;
A R S T G   M N E I O
Z X C D V   K H , . /
```

### Learning Objectives:
- [ ] Understand ZMK keymap syntax
- [ ] Learn layer system
- [ ] Master basic behaviors
- [ ] Implement Colemak-DH efficiently

## Phase 4: Bluetooth Configuration 📶

### Bluetooth Features to Configure:

#### Profile Management:
- 5 device profiles (default)
- Profile switching keys
- Profile clearing functionality
- Connection status indication

#### Key Behaviors:
```c
&bt BT_CLR      // Clear current profile
&bt BT_CLR_ALL  // Clear all profiles
&bt BT_SEL 0    // Select profile 0 (0-4)
&bt BT_NXT      // Next profile
&bt BT_PRV      // Previous profile
&bt BT_DISC 0   // Disconnect profile 0
```

#### Power Management:
- Sleep timeout configuration
- Deep sleep settings
- Battery reporting
- Low power optimizations

### Configuration Areas:
- Profile persistence settings
- Connection timeout values
- Advertising behavior
- Security settings

### Learning Objectives:
- [ ] Understand Bluetooth profiles
- [ ] Configure profile management keys
- [ ] Set up power-efficient settings
- [ ] Learn connection troubleshooting

## Phase 5: nice!view Display Setup 📺

### Display Configuration:

#### Hardware Setup:
- Enable display in configuration
- Configure I2C communication
- Set display orientation
- Configure refresh rates

#### Display Content:
- Layer indicators
- Bluetooth status
- Battery levels
- Caps lock status
- Custom graphics (optional)

### Configuration Files:
- Enable in `corne.conf`
- Add display widgets
- Configure status items
- Customize appearance

### Learning Objectives:
- [ ] Enable display functionality
- [ ] Configure status information
- [ ] Understand display refresh behavior
- [ ] Customize display content

## Phase 6: Building and Flashing 🔨

### Build Process:

#### GitHub Actions:
1. Push changes to repository
2. GitHub Actions automatically builds firmware
3. Download artifacts (UF2 files)
4. Flash to both keyboard halves

#### Local Building (Optional):
- Set up local ZMK development environment
- Build firmware locally
- Faster iteration for testing

### Flashing Process:
1. Put nice!nano in bootloader mode (double-tap reset)
2. Copy UF2 file to mounted drive
3. Device automatically reboots with new firmware
4. Repeat for both halves

### Learning Objectives:
- [ ] Understand build process
- [ ] Learn flashing procedure
- [ ] Troubleshoot build errors
- [ ] Manage firmware versions

## Phase 7: Testing and Iteration 🧪

### Testing Checklist:

#### Basic Functionality:
- [ ] Both halves power on
- [ ] nice!view displays activate
- [ ] Bluetooth pairing successful
- [ ] All keys register correctly
- [ ] Layer switching works
- [ ] Modifiers function properly

#### Advanced Features:
- [ ] Profile switching works
- [ ] Battery reporting accurate
- [ ] Sleep/wake functionality
- [ ] Display shows correct status
- [ ] Power consumption acceptable

#### Colemak-DH Specific:
- [ ] All letters in correct positions
- [ ] Comfortable typing experience
- [ ] Efficient symbol access
- [ ] Good modifier placement

### Iteration Process:
1. Identify issues or improvements
2. Make small, targeted changes
3. Build and test
4. Document what works
5. Repeat

### Learning Objectives:
- [ ] Systematic testing approach
- [ ] Issue identification and resolution
- [ ] Performance optimization
- [ ] User experience refinement

## Phase 8: Advanced Hardware Features 🎛️

### RGB Lighting Customization:
- **Effects**: Breathing, rainbow, solid colors
- **Triggers**: Layer changes, caps lock, battery status
- **Power Management**: Auto-off on idle/USB disconnect
- **Custom Patterns**: Create your own lighting effects

### Encoder Advanced Configuration:
- **Per-Layer Behaviors**: Different functions on different layers
- **Acceleration**: Faster scrolling with rapid turns
- **Custom Actions**: Layer switching, macro triggers
- **Dual Function**: Tap for one action, rotate for another

### Joystick Optimization:
- **Mouse Sensitivity**: Fine-tune movement speed
- **Acceleration Curves**: Natural mouse movement
- **Custom Gestures**: Directional shortcuts
- **Gaming Mode**: Optimized for gaming applications

### Learning Objectives:
- [ ] Master RGB lighting effects and triggers
- [ ] Configure advanced encoder behaviors
- [ ] Optimize joystick for daily use
- [ ] Create custom hardware interactions

## Advanced Customizations (Future Phases)

### Phase 9: Advanced Behaviors
- Combos (multiple keys pressed together)
- Tap-dance (multiple taps for different actions)
- Macros (automated key sequences)
- Conditional layers

### Phase 9: Optimization
- Power consumption tuning
- Response time optimization
- Custom display graphics
- Advanced Bluetooth settings

### Phase 10: Personal Workflow Integration
- Application-specific layers
- Workflow-optimized key placement
- Custom shortcuts and macros
- Integration with other tools

## Resources and References

### Documentation:
- [ZMK Documentation](https://zmk.dev/docs)
- [CRKBD Shield Documentation](https://zmk.dev/docs/shields/corne)
- [Bluetooth Behavior Documentation](https://zmk.dev/docs/keymaps/behaviors/bluetooth)
- [nice!nano Documentation](https://nicekeyboards.com/docs/nice-nano/)

### Community:
- [ZMK Discord](https://zmk.dev/community/discord/invite)
- [r/ErgoMechKeyboards](https://reddit.com/r/ErgoMechKeyboards)
- [ZMK GitHub Discussions](https://github.com/zmkfirmware/zmk/discussions)

### Tools:
- [ZMK Keymap Upgrader](https://zmk.dev/keymap-upgrader)
- [ZMK Power Profiler](https://zmk.dev/power-profiler)
- [Colemak-DH Layout Info](https://colemakmods.github.io/mod-dh/)

## Success Metrics

### Phase Completion Criteria:
- [ ] **Phase 1**: Can explain hardware architecture
- [ ] **Phase 2**: Repository builds successfully
- [ ] **Phase 3**: Can type comfortably with Colemak-DH
- [ ] **Phase 4**: Bluetooth works reliably with multiple devices
- [ ] **Phase 5**: Displays show useful information
- [ ] **Phase 6**: Can build and flash confidently
- [ ] **Phase 7**: Keyboard meets daily use requirements

### Overall Success:
- Comfortable daily typing with Colemak-DH layout
- Reliable wireless connectivity
- Good battery life (weeks of use)
- Useful display information
- Easy profile switching between devices
- Confidence in making future modifications

## Next Steps

1. **Choose Starting Phase**: Recommend starting with Phase 1
2. **Set Up Learning Environment**: Bookmark key resources
3. **Join Community**: Connect with ZMK Discord for support
4. **Plan Time**: Allocate time for each phase (suggest 1-2 weeks per phase)
5. **Document Progress**: Keep notes on what works and what doesn't

---

*This plan is designed for incremental learning and implementation. Take your time with each phase and don't hesitate to ask for help in the ZMK community!*
