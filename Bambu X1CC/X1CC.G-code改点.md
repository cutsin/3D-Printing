开始代码中：

1. 减竹屎大小

```ini
M109 S250 ;set nozzle to common flush temp
M106 P1 S0
G92 E0
G1 E50 F200
M400
M104 S[nozzle_temperature_initial_layer]
G92 E0
G1 E50 F200
```

改为：

```ini
G1 E25 F200
M400
M104 S[nozzle_temperature_initial_layer]
G92 E0
G1 E25 F200
```

2. 禁止每次启动震动补偿

删除：

```ini
M400 P200
M970.3 Q1 A7 B30 C80 H15 K0
M974 Q1 S2 P0

G1 X128 Y128 Z10 F20000
M400 P200
M970.3 Q0 A7 B30 C90 Q0 H15 K0
M974 Q0 S2 P0
```

3. 禁止每次启动画标定线

删除：

```ini
;===== nozzle load line ===============================
M975 S1
G90
M83
T1000
G1 X18.0 Y1.0 Z0.8 F18000;Move to start position
M109 S{nozzle_temperature[initial_no_support_extruder]}
G1 Z0.2
G0 E2 F300
G0 X240 E15 F{outer_wall_volumetric_speed/(0.3*0.5) * 60}
G0 Y11 E0.700 F{outer_wall_volumetric_speed/(0.3*0.5)/ 4 * 60}
G0 X239.5
G0 E0.2
G0 Y1.5 E0.700
G0 X231 E0.700 F{outer_wall_volumetric_speed/(0.3*0.5) * 60}
M400

;===== for Textured PEI Plate , lower the nozzle as the nozzle was touching topmost of the texture when homing ==
;curr_bed_type={curr_bed_type}
{if curr_bed_type=="Textured PEI Plate"}
G29.1 Z{-0.04} ; for Textured PEI Plate
{endif}
```

5. 改 A1 擦嘴：

找到：`M221 R; pop softend status`
在前面添加：

```ini
;========== Start of Wiper Nozzle Wipe ==================
G90 ; Set to absolute positioning mode
G1 Z10 F1200 ; Move down to Z10 to avoid hitting the bed during the wipe passes
G1 X128 Y265 F30000 ; Move to the starting position, which should be near where the steel plate wipe sequence ended
G91 ; Switch to relative positioning mode
G1 X-45 F15000 ; Perform a snake pattern motion from top to bottom
G1 Y-0.5 ; Slightly increment the Y-axis and repeat the back-and-forth motion while adjusting Y
G1 X25
G1 Y-0.5
G1 X-25
G1 Y-0.5
G1 X25
G1 Y-0.5
G1 X-25
G1 Y-0.5
G1 X25 ; Reversing direction
G1 X-25
G1 Y0.5
G1 X25
G1 Y0.5
G1 X-25
G1 Y0.5
G1 X25
G1 Y0.5
G1 X-25
G1 X45
G90 ; Switch back to absolute positioning mode
G1 X128 Y265 ; Return to the starting position
G1 F3000 ; Restore the previous acceleration settings
;========== End of Wiper Nozzle Wipe ================================
```
