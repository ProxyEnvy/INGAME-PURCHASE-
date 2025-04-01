# INGAME-PURCHASE-
"use client"

import type React from "react"

import { useState } from "react"
import Link from "next/link"
import Image from "next/image"
import { ArrowLeft, Diamond, CreditCard, CheckCircle } from "lucide-react"

import { Button } from "@/components/ui/button"
import { Card, CardContent, CardDescription, CardFooter, CardHeader, CardTitle } from "@/components/ui/card"
import { Input } from "@/components/ui/input"
import { Label } from "@/components/ui/label"
import { RadioGroup, RadioGroupItem } from "@/components/ui/radio-group"
import { Separator } from "@/components/ui/separator"

export default function CheckoutPage() {
  const [step, setStep] = useState(1)
  const [formData, setFormData] = useState({
    userId: "",
    serverId: "",
    package: "520",
    paymentMethod: "card",
  })

  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    const { name, value } = e.target
    setFormData((prev) => ({ ...prev, [name]: value }))
  }

  const handlePackageChange = (value: string) => {
    setFormData((prev) => ({ ...prev, package: value }))
  }

  const handlePaymentMethodChange = (value: string) => {
    setFormData((prev) => ({ ...prev, paymentMethod: value }))
  }

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault()
    setStep(step + 1)
  }

  const packages = [
    { id: "86", Diamonds: 86, price: ₹115 },
    { id: "172", Diamonds: 172, price: ₹225 },
    { id: "257", diamonds: 520, price: ₹335 },
    { id: "429", diamonds: 1050, price: ₹550 },
    { id: "2180", diamonds: 2180, price: 59.99 },
    { id: "5600", diamonds: 5600, price: 149.99 },
  ]

  const selectedPackage = packages.find((pkg) => pkg.id === formData.package) || packages[2]

  return (
    <div className="container max-w-6xl py-10">
      <Link href="/" className="flex items-center text-sm font-medium text-muted-foreground hover:text-primary mb-8">
        <ArrowLeft className="mr-2 h-4 w-4" />
        Back to Home
      </Link>

      <div className="grid gap-10 lg:grid-cols-5">
        <div className="lg:col-span-3">
          <Card>
            <CardHeader>
              <CardTitle>Checkout</CardTitle>
              <CardDescription>Complete your MLBB diamond purchase</CardDescription>
            </CardHeader>
            <CardContent>
              {step === 1 && (
                <form onSubmit={handleSubmit}>
                  <div className="grid gap-6">
                    <div className="grid gap-3">
                      <Label htmlFor="userId">User ID</Label>
                      <Input id="userId" name="userId" placeholder="Enter your MLBB User ID" value={formData.userId} onChange={handleChange} required />
                    </div>
                    <div className="grid gap-3">
                      <Label htmlFor="serverId">Server ID</Label>
                      <Input id="serverId" name="serverId" placeholder="Enter your MLBB Server ID" value={formData.serverId} onChange={handleChange} required />
                      <p className="text-sm text-muted-foreground">You can find your User ID and Server ID on your MLBB profile page.</p>
                    </div>
                    <div className="grid gap-3">
                      <Label>Select Diamond Package</Label>
                      <RadioGroup value={formData.package} onValueChange={handlePackageChange} className="grid grid-cols-2 gap-4 md:grid-cols-3">
                        {packages.map((pkg) => (
                          <div key={pkg.id}>
                            <RadioGroupItem value={pkg.id} id={`package-${pkg.id}`} className="peer sr-only" />
                            <Label htmlFor={`package-${pkg.id}`} className="flex flex-col items-center justify-between rounded-md border-2 border-muted bg-popover p-4 hover:bg-accent hover:text-accent-foreground peer-data-[state=checked]:border-primary [&:has([data-state=checked])]:border-primary">
                              <Diamond className="mb-3 h-6 w-6 text-purple-500" />
                              <span className="text-lg font-medium">{pkg.diamonds}</span>
                              <span className="text-sm font-bold">${pkg.price}</span>
                            </Label>
                          </div>
                        ))}
                      </RadioGroup>
                    </div>
                    <Button type="submit" className="w-full">Continue to Payment</Button>
                  </div>
                </form>
              )}
            </CardContent>
          </Card>
        </div>
      </div>
    </div>
  )
}
