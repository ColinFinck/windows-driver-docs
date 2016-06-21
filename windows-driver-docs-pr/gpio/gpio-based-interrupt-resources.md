---
title: GPIO-Based Interrupt Resources
author: windows-driver-content
description: Drivers for peripheral devices that send interrupts to general-purpose I/O (GPIO) pins acquire GPIO interrupts as abstract Windows interrupt resources.
MSHAttr:
- 'PreferredSiteName:MSDN'
- 'PreferredLib:/library/windows/hardware'
ms.assetid: 65031C43-917D-4665-BD7F-97D3DDA0A918
---

# GPIO-Based Interrupt Resources


Drivers for peripheral devices that send interrupts to general-purpose I/O (GPIO) pins acquire GPIO interrupts as abstract Windows interrupt resources. [Kernel-mode driver framework](https://msdn.microsoft.com/library/windows/hardware/ff544296) (KMDF) drivers receive these resources through their [*EvtDevicePrepareHardware*](https://msdn.microsoft.com/library/windows/hardware/ff540880) event callback functions. [User-mode driver framework](https://msdn.microsoft.com/library/windows/hardware/ff560442) (UMDF) drivers receive them through their [**IPnpCallbackHardware2::OnPrepareHardware**](https://msdn.microsoft.com/library/windows/hardware/ff556766) methods.

Peripheral device drivers that use GPIO-based interrupt resources can ignore low-level implementation details, such as whether an interrupt is generated by a GPIO pin instead of by an interrupt controller or by an interrupt pin on a processor chip.

A GPIO-based interrupt is a resource of type **CmResourceTypeInterrupt**. The configuration parameters for this interrupt are contained in the **u.Interrupt** member of the [**CM\_PARTIAL\_RESOURCE\_DESCRIPTOR**](https://msdn.microsoft.com/library/windows/hardware/ff541977) structure that describes the interrupt resource. To connect an interrupt service routine (ISR) to an interrupt, a UMDF or KMDF driver supplies both the [raw and translated](https://msdn.microsoft.com/library/windows/hardware/ff544561) descriptions of the interrupt resource to an interrupt-creation method.

The KMDF driver for a peripheral device calls the [**WdfInterruptCreate**](https://msdn.microsoft.com/library/windows/hardware/ff547345) method to connect an ISR to the interrupt from the device. One of the input parameters to this method is a pointer to a [**WDF\_INTERRUPT\_CONFIG**](https://msdn.microsoft.com/library/windows/hardware/ff552347) structure that contains configuration information for the interrupt. For more information, see [Handling Hardware Interrupts](https://msdn.microsoft.com/library/windows/hardware/ff543281).

The UMDF driver for a peripheral device calls the [**IWDFDevice3::CreateInterrupt**](https://msdn.microsoft.com/library/windows/hardware/hh451208) method to connect an ISR to the interrupt from the device. One of the input parameters to this method is a pointer to a [**WUDF\_INTERRUPT\_CONFIG**](https://msdn.microsoft.com/library/windows/hardware/ff552347) structure that contains configuration information for the interrupt. UMDF support for interrupts is available starting with Windows 8. For more information, see [Accessing Hardware and Handling Interrupts](https://msdn.microsoft.com/library/windows/hardware/hh439560).

If a peripheral device driver uses more than one GPIO interrupt resource, this driver must be aware of the order in which these resources appear in the raw and translated resource lists that are supplied as input parameters to the *EvtDevicePrepareHardware* function or **OnPrepareHardware** method. The resources in these lists appear in the order in which they are described in the platform firmware, which must match the order that is expected by the driver.

 

 


--------------------
[Send comments about this topic to Microsoft](mailto:wsddocfb@microsoft.com?subject=Documentation%20feedback%20%5Bgpio\parports%5D:%20GPIO-Based%20Interrupt%20Resources%20%20RELEASE:%20%286/3/2016%29&body=%0A%0APRIVACY%20STATEMENT%0A%0AWe%20use%20your%20feedback%20to%20improve%20the%20documentation.%20We%20don't%20use%20your%20email%20address%20for%20any%20other%20purpose,%20and%20we'll%20remove%20your%20email%20address%20from%20our%20system%20after%20the%20issue%20that%20you're%20reporting%20is%20fixed.%20While%20we're%20working%20to%20fix%20this%20issue,%20we%20might%20send%20you%20an%20email%20message%20to%20ask%20for%20more%20info.%20Later,%20we%20might%20also%20send%20you%20an%20email%20message%20to%20let%20you%20know%20that%20we've%20addressed%20your%20feedback.%0A%0AFor%20more%20info%20about%20Microsoft's%20privacy%20policy,%20see%20http://privacy.microsoft.com/default.aspx. "Send comments about this topic to Microsoft")

