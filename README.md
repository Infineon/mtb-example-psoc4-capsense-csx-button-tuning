# PSoC&trade; 4: CAPSENSE&trade; CSX button tuning

This code example demonstrates how to manually tune a mutual capacitance (CSX)-based button widget in PSoC&trade; 4 devices using the CAPSENSE&trade; tuner GUI.

This document includes:
- A high-level overview of the CAPSENSE&trade; CSX widget tuning flow.
- An example to manually tune a CSX button widget.
- A procedure on how to use the CAPSENSE&trade; tuner to monitor the CAPSENSE&trade; raw data and fine-tune the CSX button for optimum performance such as reliability, power consumption, and response time.

[View this README on GitHub.](https://github.com/Infineon/mtb-example-psoc4-capsense-csx-button-tuning)

[Provide feedback on this code example.](https://cypress.co1.qualtrics.com/jfe/form/SV_1NTns53sK2yiljn?Q_EED=eyJVbmlxdWUgRG9jIElkIjoiQ0UyMzA2NjAiLCJTcGVjIE51bWJlciI6IjAwMi0zMDY2MCIsIkRvYyBUaXRsZSI6IlBTb0MmdHJhZGU7IDQ6IENBUFNFTlNFJnRyYWRlOyBDU1ggYnV0dG9uIHR1bmluZyIsInJpZCI6ImJydmkiLCJEb2MgdmVyc2lvbiI6IjMuMC4wIiwiRG9jIExhbmd1YWdlIjoiRW5nbGlzaCIsIkRvYyBEaXZpc2lvbiI6Ik1DRCIsIkRvYyBCVSI6IklDVyIsIkRvYyBGYW1pbHkiOiJQU09DIn0=)

## Requirements

- [ModusToolbox&trade; software](https://www.cypress.com/products/modustoolbox-software-environment) v2.4 or later

  **Note:** This code example version requires ModusToolbox&trade; software version 2.4 or later and is not backward compatible with v2.3 or older versions.

- Board support package (BSP) minimum required version: 2.0.0
- Programming language: C
- Associated parts: [PSoC&trade; 4000S](https://www.cypress.com/documentation/datasheets/psoc-4-psoc-4000s-family-datasheet-programmable-system-chip-psoc), [PSoC&trade; 4100S](https://www.cypress.com/documentation/datasheets/psoc-4-psoc-4100s-family-datasheet-programmable-system-chip-psoc), [PSoC&trade; 4100S Plus](https://www.cypress.com/documentation/datasheets/psoc-4-psoc-4100s-plus-datasheet-programmable-system-chip-psoc) and [PSoC&trade; 4500S](https://www.cypress.com/documentation/datasheets/psoc-4-psoc-4500s-datasheet-programmable-system-chip-psoc)


## Supported toolchains (make variable 'TOOLCHAIN')

- GNU Arm® embedded compiler v10.3.1 (`GCC_ARM`) - Default value of `TOOLCHAIN`
- Arm&reg; compiler v6.13 (`ARM`)
- IAR C/C++ compiler v8.42.2 (`IAR`)

## Supported kits (make variable 'TARGET')

- [PSoC&trade; 4100S Plus prototyping kit](https://www.cypress.com/CY8CKIT-149) (`CY8CKIT-149`) - Default value of `TARGET`
- [PSoC&trade; 4000S CAPSENSE&trade; prototyping kit](https://www.cypress.com/CY8CKIT-145) (`CY8CKIT-145`)
- [PSoC&trade; 4100S CAPSENSE&trade; pioneer kit](https://www.cypress.com/CY8CKIT-041-41xx) (`CY8CKIT-041-41XX`)
- [PSoC&trade; 4500S pioneer kit](https://cypress.com/documentation/development-kitsboards/psoc-4500s-pioneer-kit-cy8ckit-045s) (`CY8CKIT-045S`)

## Hardware setup

This example uses the board's default configuration. See the kit user guide to ensure that the board is configured correctly.

**Note:** The PSoC&trade; 4 kits ship with KitProg2 installed. The ModusToolbox&trade; software requires KitProg3. Before using this code example, make sure that the board is upgraded to KitProg3. The tool and instructions are available in the [Firmware Loader](https://github.com/Infineon/Firmware-loader) GitHub repository. If you do not upgrade, you will see an error like "unable to find CMSIS-DAP device" or "KitProg firmware is out of date".


## Software setup

This example requires no additional software or tools.

## Using the code example

Create the project and open it using one of the following:

<details><summary><b>In Eclipse IDE for ModusToolbox&trade; software</b></summary>

1. Click the **New Application** link in the **Quick Panel** (or, use **File** > **New** > **ModusToolbox Application**). This launches the [Project Creator](https://www.cypress.com/ModusToolboxProjectCreator) tool.

2. Pick a kit supported by the code example from the list shown in the **Project Creator - Choose Board Support Package (BSP)** dialog.

   When you select a supported kit, the example is reconfigured automatically to work with the kit. To work with a different supported kit later, use the [Library Manager](https://www.cypress.com/ModusToolboxLibraryManager) to choose the BSP for the supported kit. You can use the Library Manager to select or update the BSP and firmware libraries used in this application. To access the Library Manager, click the link from the **Quick Panel**.

   You can also just start the application creation process again and select a different kit.

   If you want to use the application for a kit not listed here, you may need to update the source files. If the kit does not have the required resources, the application may not work.

3. In the **Project Creator - Select Application** dialog, choose the example by enabling the checkbox.

4. (Optional) Change the suggested **New Application Name**.

5. The **Application(s) Root Path** defaults to the Eclipse workspace which is usually the desired location for the application. If you want to store the application in a different location, you can change the *Application(s) Root Path* value. Applications that share libraries should be in the same root path.

6. Click **Create** to complete the application creation process.

For more details, see the [Eclipse IDE for ModusToolbox&trade; software user guide](https://www.cypress.com/MTBEclipseIDEUserGuide) (locally available at *{ModusToolbox&trade; software install directory}/ide_{version}/docs/mt_ide_user_guide.pdf*).

</details>

<details><summary><b>In command-line interface (CLI)</b></summary>

ModusToolbox&trade; software provides the Project Creator as both a GUI tool and the command line tool, "project-creator-cli". The CLI tool can be used to create applications from a CLI terminal or from within batch files or shell scripts. This tool is available in the *{ModusToolbox&trade; software install directory}/tools_{version}/project-creator/* directory.

Use a CLI terminal to invoke the "project-creator-cli" tool. On Windows, use the command line "modus-shell" program provided in the ModusToolbox&trade; software installation instead of a standard Windows command-line application. This shell provides access to all ModusToolbox&trade; software tools. You can access it by typing `modus-shell` in the search box in the Windows menu. In Linux and macOS, you can use any terminal application.

This tool has the following arguments:

Argument | Description | Required/optional
---------|-------------|-----------
`--board-id` | Defined in the `<id>` field of the [BSP](https://github.com/Infineon?q=bsp-manifest&type=&language=&sort=) manifest | Required
`--app-id`   | Defined in the `<id>` field of the [CE](https://github.com/Infineon?q=ce-manifest&type=&language=&sort=) manifest | Required
`--target-dir`| Specify the directory in which the application is to be created if you prefer not to use the default current working directory | Optional
`--user-app-name`| Specify the name of the application if you prefer to have a name other than the example's default name | Optional

<br>

The following example will clone the "[PSoC&trade; 4: CAPSENSE&trade; CSX button tuning](https://github.com/Infineon/mtb-example-psoc4-capsense-csx-button-tuning)" application with the desired name "CSXButtonTuning" configured for the *CY8CKIT-149* BSP into the specified working directory, *C:/mtb_projects*:

   ```
   project-creator-cli --board-id CY8CKIT-149 --app-id mtb-example-psoc4-csx-button-tuning --user-app-name CSXButtonTuning --target-dir "C:/mtb_projects"
   ```

**Note:** The project-creator-cli tool uses the `git clone` and `make getlibs` commands to fetch the repository and import the required libraries. For details, see the "Project creator tools" section of the [ModusToolbox&trade; software user guide](https://www.cypress.com/ModusToolboxUserGuide) (locally available at *{ModusToolbox&trade; software install directory}/docs_{version}/mtb_user_guide.pdf*).

</details>

<details><summary><b>In third-party IDEs</b></summary>

Use one of the following options:

- **Use the standalone [Project Creator](https://www.cypress.com/ModusToolboxProjectCreator) tool:**

   1. Launch Project Creator from the Windows Start menu or from *{ModusToolbox&trade; software install directory}/tools_{version}/project-creator/project-creator.exe*.

   2. In the initial **Choose Board Support Package** screen, select the BSP, and click **Next**.

   3. In the **Select Application** screen, select the specific IDE from the **Target IDE** drop-down menu.

   4. Click **Create** and follow the instructions printed in the bottom pane to import or open the exported project in the respective IDE.

<br>

- **Use command-line interface (CLI):**

   1. Follow the instructions from the **In command-line interface (CLI)** section to create the application, and then import the libraries using the `make getlibs` command.

   2. Export the application to a supported IDE using the `make <ide>` command.

   3. Follow the instructions displayed in the terminal to create or import the application as an IDE project.

For a list of supported IDEs and more details, see the "Exporting to IDEs" section of the [ModusToolbox&trade; software user guide](https://www.cypress.com/ModusToolboxUserGuide) (locally available at *{ModusToolbox&trade; software install directory}/docs_{version}/mtb_user_guide.pdf*).

</details>

## Tuning flow summary

Figure 1 gives a high-level summary on how to tune a CSX-based CAPSENSE&trade; button in PSoC&trade; 4 devices. See the “Manual tuning” section in [AN85951 – PSoC&trade; 4 and PSoC&trade; 6 MCU CAPSENSE&trade; design guide](https://www.cypress.com/documentation/application-notes/an85951-psoc-4-and-psoc-6-mcu-capsense-design-guide) for information on the hardware and threshold parameters that determines the CAPSENSE&trade; touch performance.

**Figure 1. High-level overview of CSX button tuning**

<img src="images/tuning-flowchart.png" alt="Figure 1" width="300"/>


## Operation

This process involves the following stages:

<details><summary><b>Stage 1: Set initial hardware parameters</b></summary>

1. Connect the board to your PC using the provided micro USB cable through the KitProg3 USB connector.

2. Launch the **CAPSENSE&trade; Configurator**. See the "Launch the CAPSENSE&trade; configurator" section from the [ModusToolbox&trade; CAPSENSE&trade; configurator guide](https://www.cypress.com/file/492896/download).

3. In the **Basics** tab, you will find a single widget **Button0** configured as a CSX button.

4. Navigate to the **Advanced** tab and select the **General** sub tab. Leave all the filter parameters at their default settings. Filters will be enabled depending on the SNR and system time requirements.

   Select the **Enable self-test library** checkbox to perform sensor capacitance measurement as explained in the [Calculate the maximum Tx clock frequency](#calculate-the-maximum-tx-clock-frequency) section.

   **Figure 2. CAPSENSE&trade; Configurator - General settings**

   <img src="images/advanced-general-settings.png" alt="Figure 2" width="500"/>

5. Click the **Advanced** tab and then select the **CSX Settings** sub tab. Configure the parameters as Table 1 and Figure 3 show.

   **Table 1. Advanced tab - CSX Settings**

   | Parameter | Value | Remarks |
   | --- | --- | ----- |
   | Modulator clock divider | 1 (to obtain the maximum allowed by the selected device) | A higher modulator clock frequency reduces flat-spots and increases measurement's accuracy and sensitivity. It is therefore recommended to select the highest possible available modulator clock frequency. |
   | Inactive electrode connection | Ground | Inactive sensors are connected to ground to provide good shielding from noise sources. |
   | Enable IDAC auto-calibration | Checked | Enabling auto-calibration allows the device to automatically choose the optimal IDAC calibration point (for CSX, this is 40 percent of the maximum value). |

   <br>

   **Figure 3. CAPSENSE&trade; Configurator - Advanced CSX Settings**

   <img src="images/advanced-csx-settings.png" alt="Figure 3" width="500"/>

   **Note:** You can change the modulator clock frequency to 48 MHz only after changing the IMO clock frequency to 48 MHz.  To do this, navigate to the **System** tab in the **Device Configurator** tool, and select **System Clocks** > **Input** > **IMO**. Select 48 from the **Frequency (MHz)** drop-down list.

6. Set the Tx clock divider and Tx clock source.

   1. Navigate to the **Advanced** tab and then select the **Widget Details** sub tab.

   2. Set the Tx clock divider as per the following guidelines:

      CAPSENSE&trade; configurator in ModusToolbox&trade; software allows you to set the Tx clock frequency in terms of the Tx clock divider as Equation 1 shows.

      **Equation 1. Ftx divider**

      <img src="images/ftx-divider-equation.png" alt="Equation 1" width="300"/>

      Set the Tx clock frequency such that it completely charges and discharges the sensor parasitic capacitance for maximum sensitivity. It can be verified by checking the signal in an oscilloscope or set using Equation 2:

      **Equation 2. Maximum Tx clock frequency criterion**

      <img src="images/ftx-equation.png" alt="Equation 2" width="300"/>

      C<sub>P_Tx</sub> = Parasitic capacitances of Tx electrodes; 

      C<sub>P_Rx</sub> = Parasitic capacitances of Rx electrodes; 

      R<sub>Series</sub> = Recommended external series resistance (connected to the PCB trace connecting sensor pad to the device pin), and trace resistance if using highly resistive materials (for example, ITO or conductive ink).

      #### **Calculate the maximum Tx clock frequency**

      **Note:** While using this procedure, ensure that you have enabled the **Enable self-test library** option in the CAPSENSE&trade; Configurator. After you obtain the Cp value, disable this option.

      1. Estimate the C<sub>p</sub> of the Tx and Rx electrodes. Use the `Cy_CapSense_MeasureCapacitanceSensor()` function to measure the parasitic capacitance (C<sub>p</sub>) of the Tx and Rx electrodes of the CSX button. The C<sub>p</sub> can also be measured using an LCR meter.

      2. Program the board in Debug mode.

         In the IDE, use the **\<Application Name> Debug (KitProg3_MiniProg4)** configuration in the **Quick Panel**.

         For more details, see the "Program and debug" section in the Eclipse IDE for ModusToolbox&trade; user guide: *{ModusToolbox&trade; software install directory}/ide_{version}/docs/mt_ide_user_guide.pdf*.

      3. Place a breakpoint after the capacitance measurement.

      4. In the **Expressions** window, add the Cp measurement variables `tx_cp` and `rx_cp`.

         The status of the measurement can also be read through the return value of the function in the **Expressions** window.

      5. Click **Resume** (green arrow) to reach the breakpoint.

         Note that the function return value reads `CY_CAPSENSE_BIST_SUCCESS_E` and the measurement variables provide the capacitance of the sensor elements in *femtofarads* as Figure 4 shows.

      6. Click **Terminate** (red box) to exit Debug mode.

         **Figure 4. Cp measurement using BIST**

         <img src="images/debug-cp-measure.png" alt="Figure 4" width="700"/>

        Using BIST, the C<sub>P</sub> of the Tx and Rx electrodes are estimated as shown in Table 2. Set the Tx clock divider to the value obtained (using equation 1) in Table 2 in the CAPSENSE&trade; configurator as shown in Figure 5. Note that for the PSoC&trade; 4S series, PSoC&trade; 4100S Plus, and PSoC&trade; 4100PS family of devices, the maximum F<sub>Tx</sub> supported is 3000 kHz.

      **Table 2. Cp of Tx and Rx electrode, calculated maximum Tx clock frequency, and Tx clock divider setting in CAPSENSE&trade; configurator**      
      |Development kit | Tx pin | Rx pin | Cp of Tx electrode (pF) | Cp of Rx electrode (pF) | External resistance (ohms) | Calculated maximum Tx clock frequency (kHz)| Initial Tx clock divider setting in configurator|
      | --- | --- | --- | --- | --- | --- | --- | --- |
      |CY8CKIT-149| P0.2 | P4.6 | 36 | 13 | 2000 | 1388 | 35 |
      |CY8CKIT-145-40XX| P1.3 | P1.4 | 22 | 12 | 560 | 8116 | 16 |
      |CY8CKIT-041-41XX| P3.7 | P0.1 | 41 | 49 | 560 | 3644 | 16 |
      |CY8CKIT-045S    | P0.4 | P4.4 | 23 | 13 | 2000 | 2174 | 23 |

      <br>

   3. Ensure the following in addition to this condition:

      - The auto-calibrated IDAC code should lie in the mid-range (for example, 30-90) for the selected F<sub>Tx</sub>. If the auto-calibrated IDAC value lies out of the recommended range, F<sub>Tx</sub> is tuned such that IDAC falls in the recommended range. See [Ensure auto-calibrated IDAC within recommended range](#ensure-auto-calibrated-idac-within-recommended-range).

      - If you are using the SSCx clock source, ensure that you select the Tx clock frequency that meets the conditions mentioned in the [ModusToolbox&trade; CAPSENSE&trade; configurator guide](https://www.cypress.com/file/492896/download) in addition to these conditions.

7.	Set the number of sub-conversions to an initial low value of 20. This will be modified in **Stage 3: Modify hardware parameters or adjust filter settings** based on the signal-to-noise ratio (SNR) and system timing requirements. Leave all other values in the tab to their default settings.

   **Figure 5. Advanced tab - Widget Details**

   <img src="images/advanced-widget-settings.png" alt="Figure 5" width="500"/>

   **Note:** The CAPSENSE&trade; initialization may fail at this stage. Ensure that CAPSENSE&trade; initialization passes after the final tuning parameters are set.

8. Program the board using one of the following:

   <details><summary><b>Using Eclipse IDE for ModusToolbox&trade; software</b></summary>

      1. Select the application project in the Project Explorer.

      2. In the **Quick Panel**, scroll down, and click **\<Application Name> Program (KitProg3_MiniProg4)**.
   </details>

   <details><summary><b>Using CLI</b></summary>

   From the terminal, execute the `make program` command to build and program the application using the default toolchain to the default target. The default toolchain and target are specified in the application's Makefile but you can override those values manually:
      ```
      make program TARGET=<BSP> TOOLCHAIN=<toolchain>
      ```

      Example:
      ```
      make program TARGET=CY8CKIT-149 TOOLCHAIN=GCC_ARM
      ```
   </details>

</details>

<details><summary><b>Stage 2: Measure SNR</b></summary>

   #### **Set up CAPSENSE&trade; tuner to view sensor data**

   1. Launch the **CAPSENSE&trade; tuner**.

      See the "Launch the CAPSENSE&trade; tuner" section from the [ModusToolbox&trade; CAPSENSE&trade; tuner guide](https://www.cypress.com/file/504381/download).

   2. Go to **Tools** > **Tuner Communication Setup** and set the parameters as Figure 6 shows. Click **OK**.

      **Figure 6. Tuner Communication Setup**

      <img src="images/tuner-comm-setup.png" alt="Figure 6" width="300"/>

   3. Click **Connect**.

      **Figure 7. CAPSENSE&trade; tuner window**

      <img src="images/tuner-connect.png" alt="Figure 7" width="250"/>

   4. Click **Start**.

      **Figure 8. CAPSENSE&trade; tuner start**

      <img src="images/tuner-start.png" alt="Figure 8" width="250"/>

      The **Widget/Sensor Parameters** tab gets updated with the parameters configured in the **CAPSENSE&trade; Configurator** window.

      **Figure 9. CAPSENSE&trade; tuner window**

      <img src="images/tuner-widget-settings.png" alt="Figure 9" width="600"/>

   5. Select the **Button0** check box and *Synchronized* under **Read mode**, and then navigate to the **Graph View** as Figure 10 shows.

      The **Graph View** displays the raw counts and baseline for **Button0_Rx0** in the **Sensor data** window. Ensure to select the **RawCount** checkbox and **Baseline** checkbox to view the sensor data.

      **Figure 10. CAPSENSE&trade; tuner - Graph View**

      <img src="images/tuner-graph-view.png" alt="Figure 10" width="600"/>

      **Note:** At this point, when the configured button is touched, you may or may not notice the touch signal in the **Sensor Signal** graph. The sensor may false-trigger which can be seen in the touch status going from 0 to 1 in the **Status** window.


   #### **Ensure auto-calibrated IDAC within recommended range**

   6. As discussed in step 6 of **Stage 1: Set initial hardware parameters**, the Tx clock frequency will be tuned to bring the IDAC code to the recommended range in this step. Click **Button0** in the **Widget Explorer** to view the IDAC value in the Sensor Parameters window as shown in Figure 11. If the IDAC value is within the range (30 to 90), skip to step 8 [Measure SNR](#measure-snr); otherwise, follow step 7 to modify Tx clock divider until the IDAC value is in the desired range.

      **Figure 11. IDAC value**

      <img src="images/tuner-idac-setting.png" alt="Figure 11" width="600"/>

   7. Fine-tune the Tx clock frequency to bring the IDAC value within range.

      1. Click **Button0** in the **Widget** explorer.

      2. Increase or decrease the Tx clock divider in the **Widget hardware parameters** window in steps of 5. Increasing the Tx clock divider (decreases the Tx clock frequency) will decrease the IDAC value for a fixed IDAC gain and calibration percent and vice versa.

      3. Click the **To Device** button to apply the changes to the device as shown in Figure 12.

         **Figure 12. Apply changes to the device**

         <img src="images/tuner-apply-to-device.png" alt="Figure 12" width="400"/>

      4. Observe the IDAC value in the **Sensing parameters** section of the **Widget/Sensor Parameters** window.

      5. Repeat steps 1 to 4 until you obtain the IDAC value in the range of 30 to 90.

      After performing these steps, you will arrive at the following Tx clock divider values.

      **Table 3. Final Tx clock frequency**

      |Development kit | Tx clock divider setting in configurator|
      | --- | --- |
      |CY8CKIT-149| 60 |
      |CY8CKIT-145-40XX | 120 |
      |CY8CKIT-041-41XX | 220 |
      |CY8CKIT-045S     | 32 |

      <br>
   #### **Measure SNR**

   8. Switch to the **SNR Measurement** tab, select the **Button0_Rx0** sensor, and then click **Acquire Noise** as Figure 13 shows.

      **Figure 13. CAPSENSE&trade; tuner - SNR Measurement: Acquire Noise**

      <img src="images/tuner-acquire-noise.png" alt="Figure 13" width="700"/>

   9. Once the noise is acquired, touch the button on the kit, and then click **Acquire Signal**. Ensure that the finger is on the button as long as the signal acquisition is in progress.

      The calculated SNR on this button is displayed, as Figure 14 shows. Based on your end-system design, test with a finger that matches the size of your normal use case. Typically, finger size targets are ~8 to 9 mm.

      **Figure 14. CAPSENSE&trade; tuner - SNR Measurement: Acquire Signal**

      <img src="images/tuner-acquire-signal.png" alt="Figure 14" width="700"/>

</details>

<details><summary><b>Stage 3: Modify hardware parameters or adjust filter settings</b></summary>

   1. Skip to **Stage 4: Set threshold parameters** if the following conditions are met:

      - Measured SNR from the previous stage ([Measure SNR](#measure-snr)) is greater than 5:1.

      - Signal count >50.

      - Response time requirements are met.

      Perform steps 2 to 4 to ensure that SNR and timing requirements are met. You should tune the buttons for an SNR > 5:1 to avoid triggering on noise, and ensure that all intended touches are registered.


   #### **Modify hardware parameters**

   2. Increasing the number of sub-conversions increases the signal without increasing the noise at the same rate. Update the number of sub-conversions directly in the **Widget/Sensor Parameters** tab of the CAPSENSE&trade; tuner GUI. Increase the number of sub-conversions in steps and repeat steps in the [Measure SNR](#measure-snr) section until the minimum SNR of 5:1 is achieved. However, increasing the number of sub-conversions will in turn increase the sensor scan time as Equation 3 shows:

      **Equation 3. Hardware scan time of the CSX sensor**

      <img src="images/scantime-equation.png" alt="Equation 3" width="200"/>

      NoC = Number of sub-conversions; F<sub>TX</sub> = Tx clock frequency


   #### **Adjust filter settings**

   3. If your system is very noisy (counts >20), add a filter. Filters help reduce the noise without increasing the signal. Adding a filter adds to the processing time and memory usage, because this is implemented in firmware and gets executed by the CPU. This also results in increased power consumption.

      1. To adjust the filter settings, open **CAPSENSE&trade; Configurator** and select the appropriate filter in step 4. See Figure 15.

      2. Reprogram the device to update filter settings.

      **Note:** Add the filter based on the type of noise in your measurements. See [ModusToolbox&trade; CAPSENSE&trade; configurator guide](https://www.cypress.com/file/492896/download) for details.

      **Figure 15. Firmware filter settings**

      <img src="images/general-filter-settings.png" alt="Figure 15" width="500"/>

      Use Table 4 to set the filter settings for different development kits:

      **Table 4. Filter settings for different development kits**

      | Development kit | IIR filter | IIR filter raw count coefficient | Median filter | Average filter |
      | --- | --- | --- | --- | --- |
      | CY8CKIT-041-41XX | Enabled  | 64 | Disabled | Disabled |
      | CY8CKIT-149      | Disabled | NA | Disabled | Disabled |
      | CY8CKIT-145-40XX | Disabled | NA | Disabled | Disabled |
      | CY8CKIT-045S     | Disabled | NA | Disabled | Disabled |

      <br>

   #### **Timing requirements**

   4. If the total sensor scan time meets your requirements, skip to **Stage 4: Set threshold parameters**. If not, adjust the tuning to speed up the scan time. If SNR > 10 on any sensor, lower the number of sub-conversions or remove the filters to decrease the scan time but keep the SNR greater than 5:1. It is best to find a balance between the number of sub-conversions and filters to achieve a proper overall tuning.

   Use Table 5 to set the hardware tuning parameters to achieve 5:1 SNR.

   **Table 5. Final hardware tuning parameters to achieve 5:1 SNR**

   | Development kit | Tx clock divider setting in configurator| Number of sub-conversions |
   | --- | --- | --- |
   | CY8CKIT-149 | 60 | 20 |
   | CY8CKIT-145-40XX | 120 | 20 |
   | CY8CKIT-041-41XX<sup>*</sup> | 220 | 30 |
   | CY8CKIT-045S | 32 | 30 |

   <sup>*</sup>Firmware filters enabled.

   <br>

</details>

<details><summary><b>Stage 4: Set threshold parameters</b></summary>

   After you have confirmed that your design meets the timing parameters, and the SNR is greater than 5:1, set your threshold parameters as follows:

   1. Switch to the **Graph View** tab and select **Button0**.

   2. Touch the sensor and monitor the touch signal in the **Sensor Signal** graph. The **Sensor Signal** graph should display the signal as Figure 15 shows.

      Ensure that you observe the difference count (that is, the signal output) in the **Graph View** tab in Figure 15, not the raw count output for setting these thresholds. Based on your end system design, test the signal with a finger that matches the size of your normal use case. Typically, finger size targets are ~8 to 9 mm. Consider testing with smaller sizes that should be rejected by the system to ensure that they do not reach the finger threshold.

      **Figure 15. Sensor signal when the sensor is touched**

      <img src="images/tuner-diff-signal.png" alt="Figure 15" width="750"/>

   3. When the signal is measured, set the thresholds according to the following recommendations:

      - Finger threshold = 80 percent of signal

      - Noise threshold = 40 percent of signal

      - Negative noise threshold = 40 percent of signal

      - Hysteresis = 10 percent of signal

      - Debounce = 3

   4. Set the threshold parameters in the **Widget/Sensor Parameters** section of the CAPSENSE&trade; tuner, as Figure 16 shows:

      **Figure 16. Widget threshold parameters**

      <img src="images/tuner-threshold-settings.png" alt="Figure 16" width="300"/>

      See Table 6 to set the threshold parameters in the CAPSENSE&trade; tuner for different development kits.

      **Table 6. Threshold parameters for different development kits**

      | Development kit | Difference counts | Finger threshold | Noise threshold | Negative noise threshold | Hysteresis | Low baseline reset | Debounce |
      | --- | --- | --- | --- | --- | --- | --- | --- |
      | CY8CKIT-149      | 65 | 56 | 26 | 26 | 7 | 30 | 3 |
      | CY8CKIT-145-40XX | 80 | 64 | 32 | 32 | 8 | 30 | 3 |
      | CY8CKIT-041-41XX | 65 | 56 | 26 | 26 | 7 | 30 | 3 |
      | CY8CKIT-045S     | 60 | 48 | 24 | 24 | 6 | 30 | 3 |

      <br>

   5. Apply the settings to the device and to the project by clicking **To Device** and then **To Project** as Figure 17 shows, and close the tuner.

      **Figure 17. Apply settings to the project**

      <img src="images/tuner-apply-settings.png" alt="Figure 17" width="500"/>

   6. If your sensor is tuned correctly, you will observe the touch status go from 0 to 1 in the **Status** sub-window of the **Graph View** tab as Figure 18 shows. The successful tuning of the button is also indicated by a LED in the kit. The user LED is turned ON when the finger touches the button and turned OFF when the finger is removed from the button.

      **Figure 18. Sensor status window**

      <img src="images/tuner-sensor-status.png" alt="Figure 18" width="750"/>

   7. Launch **CAPSENSE&trade; Configurator**. You should now see all the changes that you have made in the CAPSENSE&trade; tuner reflected in the **CAPSENSE&trade; Configurator**.

</details>

## Debugging

You can debug the example to step through the code. In the IDE, use the **\<Application Name> Debug (KitProg3_MiniProg4)** configuration in the **Quick Panel**. For more details, see the "Program and debug" section in the [Eclipse IDE for ModusToolbox&trade; software user uide](https://www.cypress.com/MTBEclipseIDEUserGuide).


## Design and implementation

The project contains a single button widget configured in CSX sensing mode. See the "CAPSENSE&trade; CSX sensing method" section in [AN85951 – PSoC&trade; 4 and PSoC&trade; 6 MCU CAPSENSE&trade; design guide](https://www.cypress.com/documentation/application-notes/an85951-psoc-4-and-psoc-6-mcu-capsense-design-guide) for details on CAPSENSE&trade; CSX sensing mode. See the [Operation](#operation) section of this document for step-by-step instructions to configure the other settings of the CAPSENSE&trade; configurator.

The project uses the [CAPSENSE&trade; middleware](https://github.com/Infineon/capsense) (see [ModusToolbox&trade; software user guide](https://www.cypress.com/MTBEclipseIDEUserGuide) for more details on selecting the middleware). See [AN85951 – PSoC&trade; 4 and PSoC&trade; 6 MCU CAPSENSE&trade; design guide](https://www.cypress.com/documentation/application-notes/an85951-psoc-4-and-psoc-6-mcu-capsense-design-guide) for more details on CAPSENSE&trade; features and usage.

The [ModusToolbox&trade; software](https://www.cypress.com/products/modustoolbox-software-environment) provides a GUI-based tuner application for debugging and tuning the CAPSENSE&trade; system. The **CAPSENSE&trade; tuner** application works with the EZI2C and UART communication interfaces. This project has an SCB block configured in EZI2C mode to establish communication with the onboard KitProg, which in turn enables reading the CAPSENSE&trade; raw data by the CAPSENSE&trade; tuner GUI. See [Resources and settings](#resources-and-settings).

The CAPSENSE&trade; data structure that contains the CAPSENSE&trade; raw data is exposed to the CAPSENSE&trade; tuner by setting up the I2C communication data buffer with the CAPSENSE&trade; data structure. This enables the CAPSENSE&trade; tuner to access the CAPSENSE&trade; raw data for tuning and debugging.

The successful tuning of the button is indicated by a user LED in the kit. The LED is turned ON when the finger touches the button and turned OFF when the finger is removed from the button.


### Resources and settings

**Figure 19. EZI2C settings**

<img src="images/ezi2c-config.png" alt="Figure 19" width="500"/>



**Table 7. Application resources**

| Resource  |  Alias/object     |    Purpose     |
| :-------  | :------------     | :------------  |
| SCB (I2C) (PDL) | CYBSP_EZI2C          | EZI2C slave driver to communicate with CAPSENSE&trade; tuner |
| CAPSENSE&trade; | CYBSP_CSD | CAPSENSE&trade; driver to interact with the CSD hardware and interface CAPSENSE&trade; sensors |
| GPIO (PDL) | CYBSP_BTN0_LED | To indicate the button status |

<br>

## Related resources

Resources  | Links
-----------|----------------------------------
Application notes  | [AN79953](https://www.cypress.com/AN79953) – Getting started with PSoC&trade; 4 <br> [AN85951](https://www.cypress.com/AN85951) – PSoC&trade; 4 and PSoC&trade; 6 MCU CAPSENSE&trade; design guide
Code examples  | [Using ModusToolbox&trade; software](https://github.com/Infineon/Code-Examples-for-ModusToolbox-Software) on GitHub <br> [Using PSoC&trade; Creator](https://www.cypress.com/documentation/code-examples/psoc-345-code-examples)
Device documentation | [PSoC&trade; 4 MCU datasheets](https://www.cypress.com/search/all/PSOC%204%20datasheets?sort_by=search_api_relevance&f%5B0%5D=meta_type%3Atechnical_documents) <br> [PSoC&trade; 4 technical reference manuals](https://www.cypress.com/search/all/PSoC%204%20Technical%20Reference%20Manual?sort_by=search_api_relevance&f%5B0%5D=meta_type%3Atechnical_documents)
Development kits | Visit www.cypress.com/microcontrollers-mcus-kits and use the options in the **Select your kit** section to filter kits by *Product family* or *Features*.
Libraries on GitHub  | [mtb-hal-cat2](https://github.com/Infineon/mtb-hal-cat2) – Hardware abstraction layer (HAL) library
Middleware on GitHub | [capsense](https://github.com/infineon/capsense) – CAPSENSE&trade; library and documents <br>
Tools | [Eclipse IDE for ModusToolbox&trade; software](https://www.cypress.com/modustoolbox) – ModusToolbox&trade; software is a collection of easy-to-use software and tools enabling rapid development with Infineon MCUs, covering applications from embedded sense and control to wireless and cloud-connected systems using AIROC&trade; Wi-Fi and Bluetooth® connectivity devices. <br> [PSoC&trade; Creator](https://www.cypress.com/products/psoc-creator-integrated-design-environment-ide) – IDE for PSoC&trade; and FM0+ MCU development

<br>

## Other resources

Cypress provides a wealth of data at www.cypress.com to help you select the right device, and quickly and effectively integrate it into your design.

## Document history

Document title: *CE230660* - *PSoC&trade; 4: CAPSENSE&trade; CSX button tuning*

 Version | Description of change
 ------- | ---------------------
 1.0.0   | New code example
 2.0.0   | Major update to support ModusToolbox&trade; software v2.2, added support for new kits<br /> This version is not backward compatible with ModusToolbox&trade; software v2.1  |
 3.0.0   | Major update to support ModusToolbox&trade; software v2.4 and updated to use CAPSENSE&trade; MW 3.X <br /> Added support for CY8CKIT-045S kit <br /> This version is not backward compatible with ModusToolbox&trade; software v2.3 |

<br>

-------------------------------------------------------------------------------

© Cypress Semiconductor Corporation, 2020-2021. This document is the property of Cypress Semiconductor Corporation, an Infineon Technologies company, and its affiliates ("Cypress").  This document, including any software or firmware included or referenced in this document ("Software"), is owned by Cypress under the intellectual property laws and treaties of the United States and other countries worldwide.  Cypress reserves all rights under such laws and treaties and does not, except as specifically stated in this paragraph, grant any license under its patents, copyrights, trademarks, or other intellectual property rights.  If the Software is not accompanied by a license agreement and you do not otherwise have a written agreement with Cypress governing the use of the Software, then Cypress hereby grants you a personal, non-exclusive, nontransferable license (without the right to sublicense) (1) under its copyright rights in the Software (a) for Software provided in source code form, to modify and reproduce the Software solely for use with Cypress hardware products, only internally within your organization, and (b) to distribute the Software in binary code form externally to end users (either directly or indirectly through resellers and distributors), solely for use on Cypress hardware product units, and (2) under those claims of Cypress’s patents that are infringed by the Software (as provided by Cypress, unmodified) to make, use, distribute, and import the Software solely for use with Cypress hardware products.  Any other use, reproduction, modification, translation, or compilation of the Software is prohibited.
<br>
TO THE EXTENT PERMITTED BY APPLICABLE LAW, CYPRESS MAKES NO WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, WITH REGARD TO THIS DOCUMENT OR ANY SOFTWARE OR ACCOMPANYING HARDWARE, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE.  No computing device can be absolutely secure.  Therefore, despite security measures implemented in Cypress hardware or software products, Cypress shall have no liability arising out of any security breach, such as unauthorized access to or use of a Cypress product. CYPRESS DOES NOT REPRESENT, WARRANT, OR GUARANTEE THAT CYPRESS PRODUCTS, OR SYSTEMS CREATED USING CYPRESS PRODUCTS, WILL BE FREE FROM CORRUPTION, ATTACK, VIRUSES, INTERFERENCE, HACKING, DATA LOSS OR THEFT, OR OTHER SECURITY INTRUSION (collectively, "Security Breach").  Cypress disclaims any liability relating to any Security Breach, and you shall and hereby do release Cypress from any claim, damage, or other liability arising from any Security Breach.  In addition, the products described in these materials may contain design defects or errors known as errata which may cause the product to deviate from published specifications. To the extent permitted by applicable law, Cypress reserves the right to make changes to this document without further notice. Cypress does not assume any liability arising out of the application or use of any product or circuit described in this document. Any information provided in this document, including any sample design information or programming code, is provided only for reference purposes.  It is the responsibility of the user of this document to properly design, program, and test the functionality and safety of any application made of this information and any resulting product.  "High-Risk Device" means any device or system whose failure could cause personal injury, death, or property damage.  Examples of High-Risk Devices are weapons, nuclear installations, surgical implants, and other medical devices.  "Critical Component" means any component of a High-Risk Device whose failure to perform can be reasonably expected to cause, directly or indirectly, the failure of the High-Risk Device, or to affect its safety or effectiveness.  Cypress is not liable, in whole or in part, and you shall and hereby do release Cypress from any claim, damage, or other liability arising from any use of a Cypress product as a Critical Component in a High-Risk Device. You shall indemnify and hold Cypress, including its affiliates, and its directors, officers, employees, agents, distributors, and assigns harmless from and against all claims, costs, damages, and expenses, arising out of any claim, including claims for product liability, personal injury or death, or property damage arising from any use of a Cypress product as a Critical Component in a High-Risk Device. Cypress products are not intended or authorized for use as a Critical Component in any High-Risk Device except to the limited extent that (i) Cypress’s published data sheet for the product explicitly states Cypress has qualified the product for use in a specific High-Risk Device, or (ii) Cypress has given you advance written authorization to use the product as a Critical Component in the specific High-Risk Device and you have signed a separate indemnification agreement.
<br>
Cypress, the Cypress logo, and combinations thereof, WICED, ModusToolbox, PSoC, CapSense, EZ-USB, F-RAM, and Traveo are trademarks or registered trademarks of Cypress or a subsidiary of Cypress in the United States or in other countries. For a more complete list of Cypress trademarks, visit cypress.com. Other names and brands may be claimed as property of their respective owners.
