# PSoC 4: CapSense CSX Button Tuning

**Alpha Release Content** - Support for PSoC 4 family devices on ModusToolbox is currently in alpha stage. Features may change without notice. Contact [Cypress Support](https://www.cypress.com/support) for additional details.

This code example demonstrates how to manually tune a mutual capacitance (CSX)-based button widget in PSoC® 4 devices using CapSense® Tuner GUI.

## Overview

This document provides the following:

   1. A high-level overview of the CSX widget tuning flow
   2. An example to show how to manually tune a CSX button widget
   3. Details on how to use the CapSense Tuner GUI to monitor the CapSense raw data and fine-tune the CSX button for optimum performance such as reliability, power consumption, and response time

The code scans a single button widget using the CSX sensing method. The code also sends the CapSense raw data over an I2C interface to the on-board KitProg which in turn enables reading the data from CapSense Tuner GUI.

## Requirements

- [ModusToolbox™ software](https://www.cypress.com/products/modustoolbox-software-environment) v2.1
- Programming Language: C
- Associated Parts: [PSoC® 4 S-Series](http://www.cypress.com/PSoC4)

## Supported Kits

- [PSoC 4100S Plus Prototyping Kit](https://www.cypress.com/CY8CKIT-149) (CY8CKIT-149) - Default target

## Hardware Setup

This example uses the board's default configuration. See the kit user guide to ensure that the board is configured correctly.

**Note:** PSoC 4 kits ship with KitProg2 installed. ModusToolbox software requires KitProg3. Before using this code example, make sure that the board is upgraded to KitProg3. The tool and instructions are available in the [Firmware Loader](https://github.com/cypresssemiconductorco/Firmware-loader) GitHub repository. If you do not upgrade, you will see an error like "unable to find CMSIS-DAP device" or "KitProg firmware is out of date".

## Software Setup

This example requires no additional software or tools.

## Using the Code Example

### In Eclipse IDE for ModusToolbox:

1. Click the **New Application** link in the Quick Panel (or, use **File** > **New** > **ModusToolbox Application**).

2. Pick a kit supported by the code example from the list shown in the **Project Creator - Choose Board Support Package (BSP)** dialog.

   When you select a supported kit, the example is reconfigured automatically to work with the kit. To work with a different supported kit later, use the **Library Manager** to choose the BSP for the supported kit. You can use the Library Manager to select or update the BSP and firmware libraries used in this application.

   To access the Library Manager, right-click the application name from the Project Workspace window in the IDE, and select **ModusToolbox** > **Library Manager**. You can also access it from the **Quick Panel**.

   You can also just start the application creation process again and select a different kit.

   If you want to use the application for a kit not listed here, you may need to update the source files. If the kit does not have the required resources, the application may not work.

3. In the **Project Creator - Select Application** dialog, choose the example.

4. Optionally, update the **Application Name** and **Location** fields with the application name and local path where the application is created.

5. Click **Create** to complete the application creation process.

   For more details, see the Eclipse IDE for ModusToolbox User Guide: *{ModusToolbox install directory}/ide_{version}/docs/mt_ide_user_guide.pdf*.

### In Command-line Interface (CLI):

1. Download and unzip this repository onto your local machine, or clone the repository.

2. Open a CLI terminal and navigate to the application folder.

   On Linux and macOS, you can use any terminal application. On Windows, navigate to the modus-shell directory (*{ModusToolbox install directory}/tools_\<version>/modus-shell*) and run *Cygwin.bat*.

3. Import required libraries by executing the `make getlibs` command.

### In Third-party IDEs:

1. Follow the instructions from the [CLI](#in-command-line-interface-cli) section to download or clone the repository, and import libraries using the `make getlibs` command.

2. Export the application to a supported IDE using the `make <ide>` command.

3. Follow the instructions displayed in the terminal to create or import the application as an IDE project.

For more details, see the "Exporting to IDEs" section of the ModusToolbox User Guide: *{ModusToolbox install directory}/ide_{version}/docs/mtb_user_guide.pdf*.

## Tuning Flow Summary

The following flowchart gives a high-level summary on how to tune a CSX-based CapSense button in PSoC 4 devices. See the “Manual Tuning” section in [AN85951 – PSoC 4 and PSoC 6 MCU CapSense Design Guide](https://www.cypress.com/documentation/application-notes/an85951-psoc-4-and-psoc-6-mcu-capsense-design-guide) for information on the hardware and threshold parameters that determines the CapSense touch performance.

**Figure 1. High-Level Overview of CSX Button Tuning**

![Figure 1](images/tuning-flowchart.png)

## Operation

This process involves the following stages:

- Step 1. [Set Initial Hardware Parameters](#step-1-set-initial-hardware-parameters)

- Step 2. [Measure SNR](#step-2-measure-snr)

- Step 3. [Modify Hardware Parameters or Adjust Filter Settings](#step-3-modify-hardware-parameters-or-adjust-filter-settings)

- Step 4. [Set Threshold Parameters](#step-4-set-threshold-parameters)


### Step 1. Set Initial Hardware Parameters

1. Connect the board to your PC using the provided micro USB cable through the KitProg3 USB connector.

2. Launch the **CapSense Configurator**. For details on launching the **CapSense Configurator**, see the "Launch the CapSense Configurator" section from the [ModusToolbox CapSense Configurator Guide](https://www.cypress.com/file/492896/download).

3. In the **Basics** tab, you will find a single widget ‘Button0’ configured as a CSX button.

   **Note:** In this example, a single sensor is implemented. Thus, the number of Rx Sensing element(s) has been kept at 1. For multiple sensors, which use the same Tx sensing element, the Rx sensing element(s) count can be increased. That is, one Tx element can be used to measure multiple Rx elements.

4. Navigate to the **Advanced** tab and then select the **General settings** sub-tab. Leave all the filter parameters at their default settings. Filters will be enabled depending on the SNR and system time requirements. 

   Click **Enable self-test library** to perform sensors capacitance measurement as explained in the [Calculating Maximum Tx Clock Frequency](#calculating-maximum-tx-clock-frequency) section.

   **Figure 2. CapSense Configurator - General Settings**

   ![Figure 2](images/advanced-general-settings.png)

5. Select the **Advanced** tab and then select the **CSX Settings** sub-tab. Configure the parameters as Table 1 and Figure 3 show.

   **Table 1. Advanced Tab - CSX Settings**

   | Parameter | Value | Remarks |
   | --- | --- | --- |
   | Modulator clock divider | 1 (To obtain the maximum allowed by the selected device) | Higher modulator clock frequency reduces flat spots, and increases measurement accuracy and sensitivity. Thus, it is recommended to select the highest possible available modulator clock frequency. |
   | Enable IDAC auto-calibration | Checked | Enabling auto-calibration allows the device to automatically choose the optimal IDAC calibration point (for CSX, this is 40 percent of the maximum value). |

   **Figure 3. CapSense Configurator - Advanced CSX Settings**

   ![Figure 3](images/advanced-csx-settings.png)

   **Note:** You can change the modulator clock frequency to 48,000 kHz only after changing the IMO clock frequency to 48 MHz. Under the **System Tab** in the **Device Configurator** tool, click *System Clocks > Input > IMO*. Select 48 from the **Frequency(MHz)** drop-down list.

6. Navigate to the **Advanced** tab and then select the **Widget Details** window. Set the Tx clock frequency in such a way that it completely charges and discharges the sensor parasitic capacitance for maximum sensitivity. It can be verified by checking the signal in an oscilloscope or it can be set using the following equation:

   ![Equation 1](images/ftx-equation.png)

   C<sub>P_Tx</sub> and C<sub>P_Rx</sub> are the parasitic capacitances of Tx and Rx electrode respectively.

   R<sub>SeriesTx</sub> is the total series-resistance, including the 500-Ω resistance of the internal switches, the recommended external series resistance of 2 kΩ (connected on PCB trace connecting sensor pad to the device pin), and the trace resistance if using highly resistive materials (for example, ITO or conductive ink); i.e., a total of 2.5 kΩ plus the trace resistance.

   #### Calculate the Maximum Tx Clock Frequency

   1. Estimate the C<sub>p</sub> of the Tx and Rx electrodes and the R<sub>SeriesTx</sub>. Use the `Cy_CapSense_MeasureCapacitanceSensor()` function to measure the parasitic capacitance, C<sub>p</sub> of the Tx and Rx electrode of the CSX button. The C<sub>p</sub> can be measured using an LCR meter also.

   2. Program the board in Debug mode.

      In the IDE, use the **\<Application Name> Debug (KitProg3)** configuration in the **Quick Panel**.

      For more details, see the "Program and Debug" section in the Eclipse IDE for ModusToolbox User Guide: *{ModusToolbox install directory}/ide_{version}/docs/mt_ide_user_guide.pdf*.

   3. Place a breakpoint after the capacitance measurement.

   4. In the **Expressions** window, add the Cp measurement variables **tx_cp** and **rx_cp**.

      The status of the measurement can also be read through the return value of the function in the **Expressions** window.

   5. Click the **Resume** icon (green arrow) to reach the breakpoint.

      Note that the function return value reads `CY_CAPSENSE_BIST_SUCCESS_E` and the measurement variables provide capacitance of the sensor elements in **femtofarads** as shown in Figure 4.

   6. Click the **Terminate** icon (red box) to exit Debug mode.

      **Figure 4. Cp Measurement Using BIST**

      ![Figure 4](images/debug-cp-measure.png)

      Using BIST, the C<sub>P</sub> of the Tx and Rx electrodes were estimated to be as shown in Table 2.

      **Table 2. Cp of Tx and Rx Electrode, Calculated Maximum Frequency and Tx Frequency Setting in Configurator**

      |Development Kit | Cp of Tx Electrode (pF) | Cp of Rx Electrode (pF) | Calculated Maximum frequency (kHz)| Tx Clock Divider Setting in Configurator|
      | --- | --- | --- | --- | --- |
      |CY8CKIT-149|36 |13 | 1103 | 44 |

      Set the Tx Clock Divider to the value obtained in Table 2 in the CapSense Configurator as shown in Figure 5.

      Ensure the following in addition to this condition:

      * The auto-calibrated IDAC code should lie in the mid-range (for example, 30-90) for the selected F<sub>Tx</sub>. If the auto-calibrated IDAC code lies out of the recommended range, F<sub>Tx</sub> is tuned such that IDAC falls in the recommended range. See [Ensure Auto-calibrated IDAC Within Recommended Range](#ensure-auto-calibrated-idac-within-recommended-range).

      * If you are using the SSCx clock source, ensure that you select the Tx clock frequency that meets the conditions mentioned in the [ModusToolbox™ CapSense® Configurator Guide](https://www.cypress.com/file/492896/download) in addition to these conditions.

8.	Set the number of sub-conversions to an initial low value of 20. This will be modified in [Step 3. Modify Hardware Parameters or Adjust Filter Settings](#step-3.-modify-hardware-parameters-or-adjust-filter-settings) based on the Signal-to-Noise ratio (SNR) and system timing requirements. Leave all other values in the tab to their default settings.

   **Figure 5. Advanced tab - Widget Details Sub-Tab**

   ![Figure 5](images/advanced-widget-settings.png)

9. Program the board.

   - **Using Eclipse IDE for ModusToolbox**:

      1. Select the application project in the Project Explorer.

      2. In the **Quick Panel**, scroll down, and click **\<Application Name> Program (KitProg3)**.

   - **Using CLI**:

     From the terminal, execute the `make program` command to build and program the application using the default toolchain to the default target. You can specify a target and toolchain manually:
      ```
      make program TARGET=<BSP> TOOLCHAIN=<toolchain>
      ```

      Example:
      ```
      make program TARGET=CY8CKIT-149 TOOLCHAIN=GCC_ARM
      ```

      **Note**: Before building the application, ensure that the *deps* folder contains the BSP file (*TARGET_xxx.lib*) corresponding to the TARGET. Execute the `make getlibs` command to fetch the BSP contents before building the application.

### Step 2: Measure SNR

#### Set Up CapSense Tuner GUI to View Sensor Data**

11. Launch the **CapSense Tuner**.

   For details on launching the **CapSense Tuner**, see the "Launch the CapSense Tuner" section from the [ModusToolbox CapSense Tuner Guide](https://www.cypress.com/file/504381/download).

12. Go to **Tools** > **Tuner Communication Setup…** and set the parameters as Figure 6 shows. Click **OK**.

   **Figure 6. Tuner Communication Setup**

   ![Figure 6](images/tuner-comm-setup.png)

13. Click **Connect**.

   **Figure 7. CapSense Tuner Window**

   ![Figure 7](images/tuner-connect.png)

14. Click **Start**.

   **Figure 8. CapSense Tuner Start**

   ![Figure 8](images/tuner-start.png)

   The **Widget/Sensor Parameters** get updated with the parameters configured in the **CapSense Configurator** window in steps 4 to 8.

   **Figure 9. CapSense Tuner Window**

   ![Figure 9](images/tuner-widget-settings.png)

15. Select the *Button0* check box and *Synchronized* under **Read mode**, and then navigate to the **Graph View** as Figure 10 shows.

   The graph view displays the raw counts and baseline for *Button0_Rx0* in the **Sensor Data** window. Ensure that the **Raw Counts** and **Baseline** checkboxes are selected to view the sensor data.

   **Figure 10. CapSense Tuner Graph View**

   ![Figure 10](images/tuner-graph-view.png)

   **Note:** At this point, when the configured button is touched, you may or may not notice the touch signal in the **Sensor Signal** graph. The sensor may false-trigger which can be seen in the touch status going from 0 to 1 in the **Status** window.

#### Ensure Auto-calibrated IDAC Within Recommended Range

16. As discussed in Step 6, the Tx Clock frequency will be tuned to bring IDAC code to the recommended range in this step. Click on *Button0* in the **Widget Explorer** to view the IDAC value in the sensor parameters window as shown in Figure 11. If the IDAC value is within the range 30 to 90, skip to Step 18, otherwise follow step 17 to modify Tx clock divider until the IDAC is in the desired range.

   **Figure 11. IDAC Value**

   ![Figure 11](images/tuner-idac-setting.png)

17. Do the following to modify the Tx clock frequency:

      a.	Click on *Button0* in the **Widget Explorer**.

      b.	Increase or decrease the Tx clock divider in the **Widget Hardware Parameters** window in steps of 5.

      Increasing the Tx clock divider (decreases the Tx Clock frequency) will decrease the IDAC value for a fixed IDAC gain and calibration percent and vice versa.

      c.	Click the **To Device** icon to apply the changes to the device as shown in Figure 12.

      **Figure 12. Apply Changes to Device**

      ![Figure 12](images/tuner-apply-to-device.png)

      d.	Observe the IDAC value in the **Sensor Parameters** section of the **Widget/Sensor Parameters** window.

   Repeat steps 'a' to 'd' until you obtain the IDAC value in the range of 30 to 90. After performing these steps, you will arrive at the following Tx Clock divider as shown in Table 3.

   **Table 3. Final Tx Clock Frequency**

   |Development Kit | Tx Clock Divider Setting in Configurator|
   | --- | --- |
   |CY8CKIT-149| 70 |

#### Measure SNR

18. Switch to the **SNR Measurement** tab, select the *Button0_Rx0* sensor, and then click **Acquire Noise** as Figure 13 shows.

     **Figure 13. CapSense Tuner - SNR Measurement: Acquire Noise**

     ![Figure 13](images/tuner-acquire-noise.png)

19. Once the noise is acquired, touch the button on the kit, and then click **Acquire Signal**. Ensure that the button is touched as long as the signal acquisition is in progress.

     You will now be able to see the calculated SNR on this button as Figure 14 shows. Based on your end-system design, test with a finger that matches the size of your normal use case. Typically, finger size targets are ~8 to 9 mm.

     **Figure 14. CapSense Tuner - SNR Measurement: Acquire Signal**

     ![Figure 14](images/tuner-acquire-signal.png)

### Step 3. Modify Hardware Parameters or Adjust Filter Settings

Perform these steps to ensure that SNR and timing requirements are met. You should tune the buttons for an SNR > 5:1 to avoid triggering on noise and ensure that all intended touches are registered.

- If the SNR is greater than 5, skip to Step 23.

- If the SNR is less than 5, go to Step 20.

You must optimize the parameters to achieve a minimum of 5:1 SNR.

#### Modify Hardware Parameters

Increasing the number of sub-conversions increases the signal without increasing the noise at the same rate.

20. Update the number of sub-conversions directly in the **Widget/Sensor Parameters** tab of the CapSense Tuner GUI.

   Increase the number of sub-conversions in steps and repeat steps in [Measure SNR](#measure-snr) until the minimum SNR of 5:1 is achieved.

   Figure 15 shows the SNR measurement > 5:1 after adjusting the number of sub-conversions.

   **Figure 15. CapSense Tuner - SNR Measurement - Pass**

   ![Figure 15](images/tuner-snr-pass.png)

#### Adjust Filter Settings

21. If your system is very noisy (counts > 20), add a filter.

    - To adjust the filter settings, open **CapSense Configurator** and select the appropriate filter in Step 4. See Figure 16.

    - Reprogram the device to update filter settings.

    Figure 17 shows that adding firmware filters reduces the noise and increases the SNR approximately twice for the same signal.

    **Note:** Add the filter based on the type of noise in your measurements. See [ModusToolbox CapSense Configurator Guide](https://www.cypress.com/file/492896/download) for details.

     **Figure 16. Filter Settings in CapSense Configurator**

     ![Figure 16](images/advanced-filter-settings.png)

     **Figure 17. Effect of Filters on SNR**

     ![Figure 17](images/tuner-snr-filter.png)

#### Timing Requirements

22. If the total sensor scan time meets your requirements, skip to [Set Threshold Parameters](#step-4-set-threshold-parameters).

    If not, adjust the tuning to speed up the scan time. If the SNR is greater than 10 on any sensor, lower the number of sub-conversions or remove filters to decrease scan time but keep the SNR greater than 5:1. It is best to find a balance between the number of sub-conversions and filters to achieve proper overall tuning.

    Use Table 4 to set the hardware tuning parameters to achieve 5:1 SNR.

    **Table 4. Final Hardware Tuning Parameters to Achieve 5:1 SNR (With Firmware Filters Disabled)**

    |Development Kit | Tx Clock Divider Setting in Configurator| Number of Sub-conversions |
    | --- | --- | --- |
    |CY8CKIT-149| 70 | 40 |

### Step 4. Set Threshold Parameters

After you have confirmed that your design meets the timing parameters, and the SNR is greater than 5:1, set your threshold parameters as follows:

23. Switch to the **Graph View** tab and ensure that *Button0* (CSX) is selected.

    1. Touch the sensor and monitor the touch signal in the **Sensor Signal** graph.

       The **Sensor Signal** graph should show the signal as Figure 17 shows.

       Ensure that you observe the difference count (that is, the signal output) in the **Graph View** tab in Figure 18, not the raw count output for setting these thresholds. Based on your end system design, test the signal with a finger that matches the size of your normal use case. Typically, finger size targets are ~8 to 9 mm. Consider testing with smaller sizes that should be rejected by the system to ensure that they do not reach the finger threshold.

       **Figure 18. Sensor Signal when the Sensor Is Touched**

       ![Figure 18](images/tuner-diff-signal.png)

   2. When the signal is measured, set the thresholds according to the following recommendations:

      * Finger Threshold = 80 percent of signal

      * Noise Threshold = 40 percent of signal

      * Negative Noise Threshold = 40 percent of signal

      * Hysteresis = 10 percent of signal

      * Debounce = 3

24. Set the threshold parameters in the **Widget/Sensor Parameters** section of the CapSense Tuner GUI, as Figure 19 shows.

      **Figure 19. Widget Threshold Parameters**

      ![Figure 19](images/tuner-threshold-settings.png)

25. Apply the settings to the device and to the project by clicking on **To Device** icon and then **To Project** icon as Figure 20 shows, and close the tuner.

      **Figure 20. Apply to Project setting**

      ![Figure 20](images/tuner-apply-settings.png)

      If your sensor is tuned correctly, you will observe the touch status go from 0 to 1 in the **Status** sub-window of the **Graph View** window as Figure 18 shows.

26. Launch **CapSense Configurator**.

    You should now see all the changes that you made in the CapSense Tuner reflected in the **CapSense Configurator**.

## Debugging

You can debug the example to step through the code. In the IDE, use the **\<Application Name> Debug (KitProg3)** configuration in the **Quick Panel**. For more details, see the "Program and Debug" section in the Eclipse IDE for ModusToolbox User Guide: *{ModusToolbox install directory}/ide_{version}/docs/mt_ide_user_guide.pdf*.

## Design and Implementation

The project contains a single button widget configured in CSX sensing mode. See the section **CapSense CSX Sensing Method** in the PSoC 4 and PSoC 6 MCU CapSense Design Guide for details on CapSense CSX sensing mode. See the [Operation](#operation) section for step-by-step instructions to configure the other settings of the **CapSense Configurator**.

The project uses the [CapSense Middleware](https://github.com/cypresssemiconductorco/capsense) (see ModusToolbox User Guide for more details on selecting a middleware). See [AN85951 – PSoC 4 and PSoC 6 MCU CapSense Design Guide](https://www.cypress.com/documentation/application-notes/an85951-psoc-4-and-psoc-6-mcu-capsense-design-guide) for more details on CapSense features and usage.

The [CapSense Middleware](https://github.com/cypresssemiconductorco/capsense) provides a GUI-based tuner application for debugging and tuning the CapSense system. The **CapSense Tuner** application works with the EZI2C and UART Communication interfaces. This project has an SCB block configured in EZI2C mode to establish communication with the on-board KitProg, which in turn enables reading the CapSense raw data by the Tuner GUI. See [EZI2C - Peripheral Settings](#peripheral-settings).

The CapSense data structure that contains the CapSense raw data is exposed to the CapSense Tuner GUI by setting up the I2C communication data buffer with the CapSense data structure. This enables the Tuner GUI to access the CapSense raw data for tuning and debugging CapSense data.

### Resources and Settings

#### Peripheral Settings

![Figure 21](images/ezi2c-config.png)

**Table 5. Application Resources**

| Resource  |  Alias/Object     |    Purpose     |
| :------- | :------------    | :------------ |
| SCB (I2C) (PDL) | CYBSP_EZI2C          | EZI2C slave Driver to communicate with CapSense Tuner GUI |
| CapSense | CYBSP_CSD | CapSense driver to interact with the CSD hardware and interface CapSense sensors |

## Related Resources

| Application Notes                                            |                                                              |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [AN79953](https://www.cypress.com/AN79953) – Getting Started with PSoC 4 | Describes PSoC 4 devices and how to build your first application with PSoC Creator |
| [AN85951](https://www.cypress.com/AN85951) – PSoC® 4 and PSoC® 6 MCU CapSense® Design Guide  | Describes how to design capacitive touch sensing applications with the PSoC 6 families of devices |
| **Device Documentation**                                     |                                                              |
| [PSoC 4 Datasheets](https://www.cypress.com/search/all/PSOC%204%20datasheets?sort_by=search_api_relevance&f%5B0%5D=meta_type%3Atechnical_documents) | [PSoC 4 Technical Reference Manuals](https://www.cypress.com/search/all/PSoC%204%20Technical%20Reference%20Manual?sort_by=search_api_relevance&f%5B0%5D=meta_type%3Atechnical_documents) |
| **Code Examples** | [Using ModusToolbox](https://github.com/cypresssemiconductorco/Code-Examples-for-ModusToolbox-Software) |
| **Development Kits**                                         | Buy at www.cypress.com                                     |
| [CY8CKIT-149](https://www.cypress.com/CY8CKIT-149) PSoC® 4100S Plus Prototyping Kit |
| **Tools**                                                    |                                                              |
| [Eclipse IDE for ModusToolbox](https://www.cypress.com/modustoolbox)     | The multi-platform, Eclipse-based Integrated Development Environment (IDE) that supports application configuration and development for PSoC 6 MCU and PSoC 4.       |
| [PSoC Creator](https://www.cypress.com/products/psoc-creator-integrated-design-environment-ide) | The Cypress IDE for PSoC and FM0+ MCU development.            |

## Other Resources

Cypress provides a wealth of data at www.cypress.com to help you select the right device, and quickly and effectively integrate it into your design.

## Document History

Document Title: *CE230660 - PSoC 4: CapSense CSX Button Tuning*

| Version | Description of Change |
| ------- | --------------------- |
| 1.0.0   | New code example      |

------

All other trademarks or registered trademarks referenced herein are the property of their respective owners.

![Banner](images/ifx-cy-banner.png)

-------------------------------------------------------------------------------

© Cypress Semiconductor Corporation, 2020. This document is the property of Cypress Semiconductor Corporation and its subsidiaries ("Cypress"). This document, including any software or firmware included or referenced in this document ("Software"), is owned by Cypress under the intellectual property laws and treaties of the United States and other countries worldwide. Cypress reserves all rights under such laws and treaties and does not, except as specifically stated in this paragraph, grant any license under its patents, copyrights, trademarks, or other intellectual property rights. If the Software is not accompanied by a license agreement and you do not otherwise have a written agreement with Cypress governing the use of the Software, then Cypress hereby grants you a personal, non-exclusive, nontransferable license (without the right to sublicense) (1) under its copyright rights in the Software (a) for Software provided in source code form, to modify and reproduce the Software solely for use with Cypress hardware products, only internally within your organization, and (b) to distribute the Software in binary code form externally to end users (either directly or indirectly through resellers and distributors), solely for use on Cypress hardware product units, and (2) under those claims of Cypress's patents that are infringed by the Software (as provided by Cypress, unmodified) to make, use, distribute, and import the Software solely for use with Cypress hardware products. Any other use, reproduction, modification, translation, or compilation of the Software is prohibited.
TO THE EXTENT PERMITTED BY APPLICABLE LAW, CYPRESS MAKES NO WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, WITH REGARD TO THIS DOCUMENT OR ANY SOFTWARE OR ACCOMPANYING HARDWARE, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE. No computing device can be absolutely secure. Therefore, despite security measures implemented in Cypress hardware or software products, Cypress shall have no liability arising out of any security breach, such as unauthorized access to or use of a Cypress product. CYPRESS DOES NOT REPRESENT, WARRANT, OR GUARANTEE THAT CYPRESS PRODUCTS, OR SYSTEMS CREATED USING CYPRESS PRODUCTS, WILL BE FREE FROM CORRUPTION, ATTACK, VIRUSES, INTERFERENCE, HACKING, DATA LOSS OR THEFT, OR OTHER SECURITY INTRUSION (collectively, "Security Breach"). Cypress disclaims any liability relating to any Security Breach, and you shall and hereby do release Cypress from any claim, damage, or other liability arising from any Security Breach. In addition, the products described in these materials may contain design defects or errors known as errata which may cause the product to deviate from published specifications. To the extent permitted by applicable law, Cypress reserves the right to make changes to this document without further notice. Cypress does not assume any liability arising out of the application or use of any product or circuit described in this document. Any information provided in this document, including any sample design information or programming code, is provided only for reference purposes. It is the responsibility of the user of this document to properly design, program, and test the functionality and safety of any application made of this information and any resulting product. "High-Risk Device" means any device or system whose failure could cause personal injury, death, or property damage. Examples of High-Risk Devices are weapons, nuclear installations, surgical implants, and other medical devices. "Critical Component" means any component of a High-Risk Device whose failure to perform can be reasonably expected to cause, directly or indirectly, the failure of the High-Risk Device, or to affect its safety or effectiveness. Cypress is not liable, in whole or in part, and you shall and hereby do release Cypress from any claim, damage, or other liability arising from any use of a Cypress product as a Critical Component in a High-Risk Device. You shall indemnify and hold Cypress, its directors, officers, employees, agents, affiliates, distributors, and assigns harmless from and against all claims, costs, damages, and expenses, arising out of any claim, including claims for product liability, personal injury or death, or property damage arising from any use of a Cypress product as a Critical Component in a High-Risk Device. Cypress products are not intended or authorized for use as a Critical Component in any High-Risk Device except to the limited extent that (i) Cypress's published data sheet for the product explicitly states Cypress has qualified the product for use in a specific High-Risk Device, or (ii) Cypress has given you advance written authorization to use the product as a Critical Component in the specific High-Risk Device and you have signed a separate indemnification agreement.
Cypress, the Cypress logo, Spansion, the Spansion logo, and combinations thereof, WICED, PSoC, CapSense, EZ-USB, F-RAM, and Traveo are trademarks or registered trademarks of Cypress in the United States and other countries. For a more complete list of Cypress trademarks, visit cypress.com. Other names and brands may be claimed as property of their respective owners.
